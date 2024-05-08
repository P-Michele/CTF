# Network Protocols and Commands Guide

This guide outlines several network protocols and a general command using nmap, a network scanning tool.

## Protocols

- **FTP (File Transfer Protocol):**
  - In FTP, you can attempt to log in as 'anonymous', which is the default anonymous user setting in many systems.

- **SMB (Server Message Block):**
  - SMB is a protocol used for resource sharing (files, printers, etc.) primarily on Windows systems.
  - It operates on ports 139, 137-138 (UDP) and, for secure connections, on port 445.
  - You can interact with SMB using the `smbclient` tool.

- **Redis:**
  - Redis is an open-source, in-memory data structure store, commonly used as a database.
  - It typically runs on port 6379.
  - To interact with Redis, use the `redis-cli` command line interface.

## General nmap Command

- To perform a service version detection and run default scripts against a host:
```
sudo nmap -sV -sC -oA nmap/out IP -vv -n
```
- `-sV`: Probe open ports to determine service/version info.
- `-sC`: Run default scripts.
- `-oA`: Output in all formats to the directory `nmap/out`.
- `-vv`: Increase verbosity.
- `-n`: Do not resolve DNS.

- ### Process Management

- **/proc Directory:**
- The `/proc` directory contains system and process information in real time.
- Key files and directories include:
  - `/proc/cpuinfo`: Detailed CPU information.
  - `/proc/meminfo`: Memory usage statistics.
  - `/proc/<PID>`: Process-specific information for the process with ID `<PID>`.

- **Listing Active Processes with `ps`:**
- The `ps` command provides information about running processes.
- To display all processes in a detailed manner:
  ```
  ps aux
  ```
- To analyze a specific process by its ID (PID):
  ```
  ps -p <PID> -o %cpu,%mem,cmd
  ```

- **Monitoring System Resource Usage with `top`:**
- The `top` command provides real-time system resource monitoring, displaying CPU, memory usage, and more.
- To start `top`:
  ```
  top
  ```
- Common commands within `top`:
  - `P`: Sort by CPU usage.
  - `M`: Sort by memory usage.
  - `k`: Kill a process (requires process ID).
  - `q`: Quit `top`.



