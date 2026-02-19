# Simple Port Scanner (C++ / Boost.Asio)

A high-concurrency TCP connect() port scanner written in **C++20** using **Boost.Asio**.

It classifies ports as **OPEN**, **CLOSED**, or **FILTERED** and performs basic **banner grabbing** for service fingerprinting.

> ⚠️ Scan only systems you own or have explicit permission to test.

---

## Overview

Port scanning is a foundational step in network security assessments.  
This project demonstrates:

- TCP connection mechanics (three-way handshake interpretation)
- Asynchronous I/O with Boost.Asio
- Controlled concurrency with a fixed worker model
- Timeout-based detection of filtered ports
- Basic service identification via banner reads

This is a TCP **connect scan**, meaning it performs a full TCP handshake (no raw sockets required).

---

## Features

- Asynchronous concurrent scanning
- Configurable port ranges (`1-1024`, `80-443`, etc.)
- Adjustable concurrency level
- Configurable timeout for filtered detection
- Basic service name mapping for common ports
- Banner grabbing (reads up to 128 bytes after connect)
- Summary statistics after scan completion

---

## Technologies Used

- **C++20**
- **Boost.Asio** (networking + async runtime)
- **Boost.Program_options** (CLI parsing)
- **CMake** (build system)

---

## Project Structure

```text
simple-port-scanner/
├── CMakeLists.txt
├── include/
│   └── PortScanner.hpp
├── src/
│   ├── PortScanner.cpp
│   └── main.cpp
└── README.md
```

## How It Works

For each port:

1. Start an asynchronous `connect()` attempt.
2. Start a `steady_timer` with the configured timeout.
3. Whichever finishes first determines the state:

   - Connect succeeds → **OPEN**
   - Connect fails immediately (RST) → **CLOSED**
   - Timer expires before connection completes → **FILTERED**

Concurrency is limited by a fixed worker count (`-t`).  
Completion handlers pull the next port from the queue, maintaining constant parallelism.

---

## Requirements

- C++20 compatible compiler (GCC 10+, Clang 12+, MSVC 2019+)
- CMake ≥ 3.16
- Boost (with `program_options`)

---

## Install Boost

### Ubuntu / Debian

```bash
sudo apt update
sudo apt install -y cmake g++ libboost-all-dev
```
### Install (macOS)

```bash
brew install cmake boost
```
### build 
``` bash
mkdir -p build
cd build
cmake ..
make
```
Executable:
```
build/simplePortScanner
```
## Usage
### Basic scan (default behavior)

```
./simplePortScanner -i 127.0.0.1 -p 1-1024 -t 100 -e 2
```
### Scan a full port range
```
./simplePortScanner -i 192.168.1.1 -p 1-65535 -t 200
```
### Increase timeout for high-latency targets
```
./simplePortScanner -i example.com -p 80-443 -e 5
```
## CLI Options 
| Flag | Description                     | Default     |
| ---- | ------------------------------- | ----------- |
| `-i` | Target IP or domain             | `127.0.0.1` |
| `-p` | Port range (`N` or `start-end`) | `1-1024`    |
| `-t` | Max concurrent connections      | `100`       |
| `-e` | Timeout in seconds              | `2`         |
| `-h` | Help message                    | —           |

## Important

Port lists such as `22,80,443` are **not supported**.

Supported formats:

- `1024` → scans `1–1024`
- `80-443` → scans range
- `1-65535` → full scan

---

## Example Output

```text
PORT    STATE     SERVICE   BANNER
22      OPEN      SSH       SSH-2.0-OpenSSH_8.9p1
80      CLOSED    HTTP      ---
443     FILTERED  NULL      NULL

Result:
  Open ports: 1
  Closed ports: 1
  Filtered ports: 1
```

## Design Decisions

### Connect scan instead of SYN scan

- No root privileges required  
- Cross-platform compatibility  
- Cleaner integration with Boost.Asio  

### Timer-based filtered detection

- Works without raw sockets  
- Distinguishes silent packet drops from explicit rejections  

### Strand for thread safety

- Prevents race conditions in shared state  
- Serializes completion handlers without manual mutexes  

---

## Future Improvements

- Support comma-separated port lists  
- Add UDP scan mode  
- Add SYN scan mode (raw sockets)  
- Structured output (JSON/CSV)  
- Rate limiting  
- Unit tests for port parsing and state handling  


