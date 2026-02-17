
                                                                                 



# Simple Port Scanner (C++)

An asynchronous TCP port scanner implemented in C++ using boost::asio

## What It Does

- Asynchronous TCP port scanning using Boost.Asio for high concurrency
- Configurable port ranges from single ports to full 65535 scans
- Adjustable concurrency level to control scan speed and network load
- Connection timeout configuration to handle filtered ports gracefully
- Clean terminal output showing open, closed, and filtered port states
## Features
- Configurable port ranges (e.g. 1-1024, 1-65535)
- Adjustable concurrency level
- Connection timeouts
- Clean and readable CLI output

## Educational Value
- Asynchronous IO using boost::asio
- TCP socket programming
- Concurrency control
- Basic network reconnaissance techniques

## Build

### Requirements
- C++20
- Boost library
- CMake >= 3.16

### Build Instructions
```bash
mkdir build && cd build
cmake ..
make
```
## Usage

```bash
./port_scanner -i 127.0.0.1 -p 1-1024 -t 100 -e 2
