# USB Simulation Documentation

## Overview

**Project Name:** USB Simulation  
**Purpose:** Simulate USB communication using virtual serial ports for development and testing.

## Tools and Setup

**Tools Used:**
- `socat`: A command-line based utility that creates virtual serial ports.
- `/dev/pts/` Directory: Represents the virtual serial ports.

**System Requirements:**
- Linux operating system (Ubuntu or similar)
- `socat` installed

**Installation:**
1. **Install `socat`:**
   ```bash
   sudo apt-get install socat
   ```
## Creating Virtual Serial Ports
2 .Creating Virtual Serial Ports
Command to Create Virtual Ports:

``bash

`socat -d -d pty,raw,echo=0 pty,raw,echo=0 &
```
Output:

```less
2024/09/07 18:27:26 socat[7620] N PTY is /dev/pts/1
2024/09/07 18:27:26 socat[7620] N PTY is /dev/pts/3
2024/09/07 18:27:26 socat[7620] N starting data transfer loop with FDs [5,5] and [7,7]
```
## Explanation:
This command creates two virtual serial ports: /dev/pts/1 and /dev/pts/3.
socat will handle data transfer between these ports, allowing for communication simulation.
Testing Communication
1. Listening on One Port: Open a terminal and run:

```bash
```cat /dev/pts/1
```
This will display any data sent to /dev/pts/1.

2. Sending Data from the Other Port: Open another terminal and run:

```bash
echo "Hello from /dev/pts/3" > /dev/pts/3
```
This sends a message to /dev/pts/3, which should appear in the terminal where you are listening to /dev/pts/1.

## Integration with Application
Using Virtual Ports in Application:

Configure your application to use /dev/pts/1 and /dev/pts/3 as if they were USB devices.
Implement logic to handle data read and write operations on these virtual serial ports.
Example Python Script:

```python

import serial

# Open serial ports
port_in = serial.Serial('/dev/pts/1', 9600)
port_out = serial.Serial('/dev/pts/3', 9600)

try:
    while True:
        # Read from input port
        data = port_in.readline()
        # Write to output port
        port_out.write(data)
except KeyboardInterrupt:
    pass
finally:
    port_in.close()
    port_out.close()
```
## Troubleshooting
Common Issues:

Ports Not Available: Ensure socat is running and that no other process is using the ports.
Permissions Issues: Verify you have the necessary permissions to access /dev/pts/.
Stopping socat:

Find the process ID:
```bash

ps aux | grep socat
```
Kill the process:
```bash
kill <process_id>
```
### Conclusion
This setup allows you to simulate USB communication using virtual serial ports. This can be useful for testing and development of USB-related applications in a controlled environment without physical hardware.
