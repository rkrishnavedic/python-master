# Detailed Linux System Administration Notes

## File Management Commands

### ls (List Directory Contents)
The `ls` command is used to list directory contents.

Basic usage:
```bash
ls
```

Useful options:
- `ls -l`: Long format, showing permissions, owner, size, and date
- `ls -a`: Show all files, including hidden ones (starting with .)
- `ls -t`: Sort by modification time, newest first
- `ls -r`: Reverse order
- `ls -R`: Recursive, list subdirectories

Common combinations:
```bash
ls -ltr  # Long format, sorted by time, reverse order
ls -la   # Long format, including hidden files
```

Example output of `ls -ltr`:
```
-rw-r--r-- 1 user group  2048 Jul  1 10:00 file3.txt
-rw-r--r-- 1 user group  4096 Jul  2 15:30 file2.txt
-rw-r--r-- 1 user group  8192 Jul  3 09:45 file1.txt
```

### cd (Change Directory)
The `cd` command is used to change the current working directory.

Examples:
```bash
cd /home/user/Documents
cd ..  # Move up one directory
cd ~   # Move to home directory
cd -   # Move to previous directory
```

### mkdir (Make Directory)
Creates new directories.

Examples:
```bash
mkdir new_folder
mkdir -p parent/child/grandchild  # Create parent directories as needed
```

### rm (Remove)
Removes files and directories.

Examples:
```bash
rm file.txt
rm -r directory  # Remove directory and its contents
rm -f file.txt   # Force removal without prompting
```

### cp (Copy)
Copies files and directories.

Examples:
```bash
cp source.txt destination.txt
cp -r source_dir destination_dir  # Copy directory recursively
```

### mv (Move)
Moves or renames files and directories.

Examples:
```bash
mv old_name.txt new_name.txt  # Rename file
mv file.txt /path/to/new/location/  # Move file
```

### chmod (Change Mode)
Changes file permissions.

Examples:
```bash
chmod 644 file.txt  # rw-r--r--
chmod +x script.sh  # Add execute permission
chmod -w file.txt   # Remove write permission
```

### chown (Change Owner)
Changes file ownership.

Example:
```bash
chown user:group file.txt
```

### find
Searches for files and directories.

Examples:
```bash
find /home/user -name "*.txt"
find . -type d  # Find directories
find . -mtime -7  # Files modified in the last 7 days
```

### gzip and gunzip
Compression and decompression utilities.

Examples:
```bash
gzip large_file.txt  # Compresses to large_file.txt.gz
gunzip large_file.txt.gz  # Decompresses back to large_file.txt
gzip -9 file.txt  # Maximum compression
gzip -l file.txt.gz  # List contents of compressed file
```

## User Management

### useradd
Creates a new user.

Example:
```bash
sudo useradd -m -s /bin/bash newuser
```

### userdel
Deletes a user.

Example:
```bash
sudo userdel -r username  # -r removes home directory
```

### passwd
Changes user password.

Example:
```bash
sudo passwd username
```

### usermod
Modifies user account.

Examples:
```bash
sudo usermod -aG sudo username  # Add user to sudo group
sudo usermod -s /bin/bash username  # Change user's shell
```

### groupadd and groupdel
Manage groups.

Examples:
```bash
sudo groupadd newgroup
sudo groupdel oldgroup
```

## Bash Programming

### Variables
```bash
NAME="John"
echo "Hello, $NAME"
```

### Conditionals
```bash
if [ "$1" = "hello" ]; then
    echo "Hello to you too!"
elif [ "$1" = "bye" ]; then
    echo "Goodbye!"
else
    echo "I don't understand"
fi
```

### Loops
```bash
# For loop
for i in {1..5}; do
    echo "Number: $i"
done

# While loop
count=0
while [ $count -lt 5 ]; do
    echo "Count: $count"
    ((count++))
done
```

### Functions
```bash
greet() {
    echo "Hello, $1!"
}

greet "World"
```

### Debugging

```bash
#!/bin/bash -x  # Enable debugging for entire script

set -x  # Enable debugging from this point
# debugging-enabled code
set +x  # Disable debugging

# Use 'set -e' to exit on error
set -e
```

## Networking Concepts

### IP Addressing
- IPv4: 32-bit address (e.g., 192.168.1.1)
- IPv6: 128-bit address (e.g., 2001:0db8:85a3:0000:0000:8a2e:0370:7334)

### Subnets and CIDR
- Subnet mask: 255.255.255.0 (for a /24 network)
- CIDR notation: 192.168.1.0/24

### DNS (Domain Name System)
Translates domain names to IP addresses.

Example DNS lookup:
```bash
nslookup google.com
```

```bash
<<Computer Networks
 1. IP Addresses 
    IPv4 addresses are in decimal notations 192.168.19.208
        - all are in 8 bits notation (8bits).(8bits).(8bits).(8bits)
        - 8 bits ranges from 00000000 -> 11111111 (in binary) i.e. 0 -> 255 (in decimals)
        - So, there can be 2^32 (~4.3 billion) IP addresses
    IPv6 addresses are in hexadecimal (base 16) notations 3001:0da8:75a3:0000:0000:8a2e:0370:7334 (8 parts each having 4 hexadecimal digits)
        - (8 parts)*(4 hexadecimal digit)*(4 bits required to represent hexadecimal digit) = 128 bits
        - 2^128 is quite huge (~3.4e+38)
    NAT : Network Address Translation
        - There public and private IP addresses. Private IP addresses are for internal use and talks to single public IP address if required.
    MAC Address (Medium Access Control)
        - Device unique identifier address
 2. Protocols and Models
    TCP
        - SYN > SYN ACK -> ACK (3way handshake)
        - Common Ports: FTP 21, SSH 22, Telnet 23, SMTP  25, DNS 53, HTTP 80, HTTPS 443, POP3 110, SMB 139+445, IMAP 143
    UDP
        - DNS 53, DHCP 67,68, TFTP 69, SNMP 161
    OSI Model
    The OSI (Open Systems Interconnection) model is a conceptual framework used to understand network interactions in seven layers:
        1. Physical Layer: Hardware connections (cables, switches).
        2. Data Link Layer: Direct node-to-node data transfer (MAC addresses, switches).
        3. Network Layer: Path determination and IP addressing (routers).
        4. Transport Layer: Data transfer management (TCP/UDP).
        5. Session Layer: Establishes, manages, and terminates connections.
        6. Presentation Layer: Data translation and encryption.
        7. Application Layer: Network services to end-users (HTTP, FTP).
 3. Subnetting
    netmask looks like this 255.255.255.0/24
        - CIDR Range
        - Wildcard bits 
Computer
```
-- stop here session 1

### DHCP (Dynamic Host Configuration Protocol)
Automatically assigns IP addresses to devices on a network.

### OSI Model
7 layers: Physical, Data Link, Network, Transport, Session, Presentation, Application

### TCP/IP Model
4 layers: Network Interface, Internet, Transport, Application

## Network Devices

### Switches
Layer 2 devices that forward traffic based on MAC addresses.

### Routers
Layer 3 devices that forward traffic between different networks based on IP addresses.

### Firewalls
Security devices that control incoming and outgoing network traffic.

## Network Protocols

### SSH (Secure Shell)
Secure remote access protocol.

Example:
```bash
ssh user@remote_host
```

### SMTP (Simple Mail Transfer Protocol)
Email transmission protocol.

### HTTP/HTTPS
Web browsing protocols.

## Interview Topics

### Linux File System Hierarchy
- `/`: Root directory
- `/home`: User home directories
- `/etc`: System configuration files
- `/var`: Variable data (logs, temp files)
- `/bin`, `/sbin`: Essential system binaries

### Process Management
- `ps`: List processes
- `top`: Dynamic view of system processes
- `kill`: Terminate processes

### System Monitoring and Logging
- `top`: Real-time system monitor
- `/var/log`: System log files
- `dmesg`: Kernel ring buffer

### Virtualization and Containerization
- Virtual Machines (VMs)
- Containers (Docker, Kubernetes)

### Network Troubleshooting
- `ping`: Test network connectivity
- `traceroute`: Show route to destination
- `netstat`: Network statistics

### Security Best Practices
- Regular system updates
- Strong password policies
- Principle of least privilege
- Firewall configuration

### Version Control (Git)
- `git init`: Initialize repository
- `git add`: Stage changes
- `git commit`: Record changes
- `git push`: Upload local changes to remote
- `git pull`: Download and merge remote changes
