# Fuzzing Tools

## Directory Brute Forcing

- **Dirsearch:**
  - A simple yet effective directory brute-forcing tool.
  - Usage:
    ```bash
    dirsearch.py -u https://10.10.10.103/ -e aspx,txt
    ```

- **Dirbuster:**
  - A GUI-based tool to brute force directories and files on a web server.

- **Dirb:**
  - A tool that launches dictionary attacks against a website to identify hidden directories and files.
  - Usage:
    ```bash
    dirb http://10.10.10.103/ /path/to/wordlist.txt
    ```

- **Eyewitness:**
  - Captures screenshots of web pages and collects information for review.
  - Usage:
    ```bash
    eyewitness -f urls.txt --web --no-prompt
    ```

- **Altdns:**
  - Allows the discovery of subdomains based on known patterns.
  - Usage:
    ```bash
    altdns -i subdomains.txt -o results_output.txt -w words.txt
    ```

## Port Scanning

- **Masscan:**
  - An extremely fast port scanner.
  - Usage:
    ```bash
    masscan -p1-65535 10.10.10.103 --rate 1000
    ```

- **Nmap:**
  - An advanced network scanner for service and version detection.
  - Usage:
    ```bash
    sudo nmap -sV -sC -oA nmap/initial 10.10.10.103 -vv -n
    ```
  - Key flags:
    - `-sV`: Version detection.
    - `-sC`: Default scripts.
    - `-oA`: Output in all formats.

## Username Generation

- **Username-Anarchy:**
  - Generates all possible username combinations.

## Web Reconnaissance

- **WhatWeb:**
  - Identifies the framework and technology used by a web application.
  - Usage:
    ```bash
    whatweb -a 3 http://10.10.10.103
    ```

- **Virtual-Host-Discovery:**
  - Enumerates hosts on a given IP address.
  - Usage:
    ```bash
    virtual-host-discovery -t http://10.10.10.103
    ```

- **Arjun:**
  - Discovers HTTP parameters by analyzing web application behavior.
  - Usage:
    ```bash
    arjun -u https://10.10.10.103 -m POST
    ```
