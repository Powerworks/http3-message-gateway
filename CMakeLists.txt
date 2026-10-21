cmake_minimum_required(VERSION 3.10)
project(http3_message_gateway C)

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)

add_executable(gateway
    src/gateway/msg_frame.c
    src/gateway/session.c
    src/gateway/load_balancer.c
    src/gateway/gateway.c
    src/gateway/gateway_main.c
)

target_include_directories(gateway PRIVATE src/gateway)
