cmake_minimum_required(VERSION 3.19)
project(daytime_RFC_867 C)

set(CMAKE_C_STANDARD 11)

include_directories(.)
link_libraries(pthread)

add_executable(daytime_RFC_867_Server
        server.c
        server.h)

add_executable(daytime_RFC_867_Client
        client.c
        client.h)
