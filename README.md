# BHP Net Tool (Python Netcat Clone)

This is a Python implementation of a Netcat-like tool, inspired by Black Hat Python. It allows you to send and receive data over TCP, execute commands remotely, upload files, and open a command shell on a remote machine. This tool is useful for network debugging, penetration testing, and learning about socket programming in Python.

---

## Table of Contents
- [Features](#features)
- [Requirements](#requirements)
- [Usage](#usage)
- [Command-Line Options](#command-line-options)
- [Examples](#examples)
- [Feature Breakdown](#feature-breakdown)
- [How It Works (Architecture)](#how-it-works-architecture)
- [Troubleshooting](#troubleshooting)
- [Security Considerations](#security-considerations)
- [Notes](#notes)
- [License](#license)

---

## Features
- **Connect to a remote host and port**: Act as a client to send data or interact with a server.
- **Listen for incoming connections**: Act as a server, accepting connections from clients.
- **Execute commands on the remote system**: Run a specified command upon connection and send the output back.
- **Upload files to the remote system**: Receive a file from the client and save it to disk.
- **Interactive command shell**: Open a shell on the server, allowing the client to execute arbitrary commands interactively.

---

## Requirements
- Python 3.x

No external dependencies are required (uses only Python standard library).

---

## Usage
Run the script from the command line. You can either connect to a remote host or listen for incoming connections. The tool supports both sending data and interactive sessions.

### Command-Line Options
```
usage: netcat.py [-h] [-c] [-e EXECUTE] [-l] [-p PORT] [-t TARGET] [-u UPLOAD]

optional arguments:
  -h, --help            show this help message and exit
  -c, --command         command shell
  -e EXECUTE, --execute EXECUTE
                        execute specified command
  -l, --listen          listen
  -p PORT, --port PORT  specified port (default: 5555)
  -t TARGET, --target TARGET
                        specified IP (default: 244.178.44.111)
  -u UPLOAD, --upload UPLOAD
                        upload file
```

---

## Examples
- **Command shell (listener):**
  ```
  python netcat.py -t 192.168.1.108 -p 5555 -l -c
  ```
- **Upload a file:**
  ```
  python netcat.py -t 192.168.1.108 -p 5555 -l -u=mytest.txt
  ```
- **Execute a command:**
  ```
  python netcat.py -t 192.168.1.108 -p 5555 -l -e="cat /etc/passwd"
  ```
- **Send data to a server:**
  ```
  echo 'ABC' | python netcat.py -t 192.168.1.108 -p 135
  ```
- **Connect to a server:**
  ```
  python netcat.py -t 192.168.1.108 -p 5555
  ```

---

## Feature Breakdown
- **-l / --listen**: Start the tool in server mode, waiting for incoming connections.
- **-t / --target**: Specify the target IP address (for both client and server modes).
- **-p / --port**: Specify the port to connect to or listen on.
- **-c / --command**: Open a command shell on the server for the client to interact with.
- **-e / --execute**: Run a specific command on the server upon connection and send the output to the client.
- **-u / --upload**: Save all received data to a file with the given name on the server.

---

## How It Works (Architecture)
- The script uses Python's `socket` library to create TCP connections.
- In **listen mode**, it binds to the specified IP and port, accepting incoming connections and spawning a new thread for each client.
- In **client mode**, it connects to the specified server and port, optionally sending data from stdin.
- The **command shell** feature allows the client to execute arbitrary commands on the server interactively.
- The **upload** feature writes all received bytes to a file on the server.
- The **execute** feature runs a single command on the server and returns the output.
- All features are implemented using threads to allow multiple simultaneous connections.

---

## Troubleshooting
- **"Can't open file" error:** Make sure you are running the script from the correct directory or provide the full path to `netcat.py`.
- **Permission denied:** Run the script with appropriate permissions, especially when binding to ports < 1024 or writing files.
- **Firewall issues:** Ensure that your firewall allows the chosen port for inbound/outbound connections.
- **No response from server:** Double-check the target IP, port, and that the server is running and listening.
- **Python version issues:** This script requires Python 3.x. Check your Python version with `python --version`.

---

## Security Considerations
- **Do not use on untrusted networks.**
- **Do not expose to the public internet.**
- The command shell and execute features allow arbitrary code execution and can be extremely dangerous if misused.
- Use only in controlled environments and with explicit permission.
- Uploaded files are written directly to disk without validation—be cautious of overwriting important files.

---

## Notes
- The default target IP is set to `244.178.44.111`. Change this as needed.
- Only TCP is supported.
- Use responsibly and only on systems you have permission to access.

---

## License
This project is for educational purposes only. 