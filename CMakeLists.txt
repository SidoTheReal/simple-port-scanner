cmake_minimum_required(VERSION 3.16)
project(simplePortScanner LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

find_package(Boost REQUIRED COMPONENTS program_options)

add_executable(simplePortScanner
    src/main.cpp
    src/PortScanner.cpp
)

target_include_directories(simplePortScanner PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

target_link_libraries(simplePortScanner PRIVATE
    Boost::program_options
)
