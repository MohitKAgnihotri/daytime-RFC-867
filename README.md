# Title: Daytime Server 
## Program requirements: 
- Allow clients to connect using port number 1313 and TCP protocol
- Send the current date & time to the connected client.
- Server closes the TCP socket for the client once it has sucessfully transmitted the message
- Server should handle SIG_TERM and ensure the server socket is closed.
- Server should be able to handle multiple clients simultaneously. 
- There is NO limit on the number of simultaneous clients unless limited by the system resources.

## Program inputs: 
There is NO input required for the server. It runs on a well-defined port 1313
## Program outputs: 
There is NO specific output for the server. It runs as a deamon and allows the connected clients to reterive the day & time. 

## Test plan: 
- Ensure server is running and client can connect to it.
```
expected input: Single client connect
expected output: Server sends day & time to each and continues to operate.
```
- Ensure more than one client can connect simultaneously. 
```
expected input: Multiple client connect
expected output: Server sends day & time to each and continues to operate.
```
- Test server output using nc command:
```
expected input: echo "ignored" |nc localhost 1313
expected output: Date and Time for example: Sat Feb 13 14:02:02 2021
```
- Execute server with the valgrind.
```
expected input: ^C
expected output: 
==6928== Memcheck, a memory error detector
==6928== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==6928== Using Valgrind-3.15.0 and LibVEX; rerun with -h for copyright info
==6928== Command: ./daytime_RFC_867_Server
==6928==
==6928== error calling PR_SET_PTRACER, vgdb might block
Client connected
^C==6928==
==6928== HEAP SUMMARY:
==6928==     in use at exit: 0 bytes in 0 blocks
==6928==   total heap usage: 11 allocs, 11 frees, 10,459 bytes allocated
==6928==
==6928== All heap blocks were freed -- no leaks are possible
==6928==
==6928== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```


## Design overview: 
### Steps to create a server:
- Create a socket and bind it to the addrees and Port=1313
- Start listening to the socket. 
- Setup signal handler for SIG_TERM
- Loop while (1)
-- accept new_connection
-- create a new thread to handle new_connection
- goto step 3


# Title: Daytime Client

## Program requirements: 
- Connect to a given daytime server using RFC 867
- Connect to a given daytime server using TCP Protocol
- Print correctly date & time sent.

## Program inputs: 
There is 2 input required for the client. 
- First is the name or the IP address of the server. 
- Second is the port number for the server
```
	Example of the input: ./daytime_RFC_867_Client localhost 1313
						  ./daytime_RFC_867_Client 127.0.0.1 1313
```
## Program outputs: The program prints the current day & time as reported by the server. 
	Example output: Sat Feb 13 14:13:09 2021

## Test plan: 
- Ensure server is running and client can connect to it.
```
expected input: Single client connect
expected output: Client prints the response from the server and exits.
```
- Ensure more than one client can connect simultaneously. 
```
expected input: Multiple client connect
expected output: Client prints the response from the server and exits.
```
- Test client using standard server:
```
expected input: ./daytime_RFC_867_Client localhost 13
expected output: Date and Time for example: Sat Feb 13 14:02:02 2021
```
- Execute client with the valgrind.
```
expected input: ./daytime_RFC_867_Client localhost 1313
expected output: 
==6945== Memcheck, a memory error detector
==6945== Copyright (C) 2002-2017, and GNU GPL'd, by Julian Seward et al.
==6945== Using Valgrind-3.15.0 and LibVEX; rerun with -h for copyright info
==6945== Command: ./daytime_RFC_867_Client 127.0.0.1 1313
==6945==
==6945== error calling PR_SET_PTRACER, vgdb might block
Sat Feb 13 14:16:58 2021
==6945==
==6945== HEAP SUMMARY:
==6945==     in use at exit: 0 bytes in 0 blocks
==6945==   total heap usage: 16 allocs, 16 frees, 14,986 bytes allocated
==6945==
==6945== All heap blocks were freed -- no leaks are possible
==6945==
==6945== ERROR SUMMARY: 0 errors from 0 contexts (suppressed: 0 from 0)
```

## Design overview: 
### Steps to create a client:
- Create a socket and bind it to the addrees and Port=1313
- Start listening to the socket. 
- Setup signal handler for SIG_TERM
- connect to server using IP_Addr & Port_Num
- Receive message from the server
- Print the message 
- exit
