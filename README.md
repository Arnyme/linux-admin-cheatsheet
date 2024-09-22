# Comprehensive Linux System Administrator Cheatsheet

## 1. Basic Commands and File System Navigation

### Listing Files and Directories
```bash
# List files in the current directory
ls

# List all files (including hidden ones) with details
ls -la

# List files, sorted by size
ls -lS

# List files, sorted by modification time
ls -lt
```

### Navigating Directories
```bash
# Change to home directory
cd ~

# Change to parent directory
cd ..

# Change to previous directory
cd -

# Print working directory
pwd
```

### File and Directory Operations
```bash
# Create a directory
mkdir new_directory

# Create nested directories
mkdir -p parent/child/grandchild

# Remove empty directory
rmdir empty_directory

# Remove file
rm file.txt

# Remove directory and its contents (use with caution!)
rm -r directory_name

# Copy file
cp source.txt destination.txt

# Copy directory and its contents
cp -r source_dir destination_dir

# Move/rename file or directory
mv old_name new_name

# Create an empty file or update timestamp of existing file
touch file.txt
```

## 2. File Editing and Text Processing

### Text Editors
```bash
# Open file in nano
nano file.txt

# Open file in vim
vim file.txt

# Open file in emacs
emacs file.txt
```

### Text Processing
```bash
# Search for a pattern in a file
grep "pattern" file.txt

# Search recursively in directories
grep -r "pattern" /path/to/directory

# Count lines, words, and characters in a file
wc file.txt

# Display first 10 lines of a file
head file.txt

# Display last 10 lines of a file
tail file.txt

# Follow the growth of a log file in real-time
tail -f /var/log/syslog
```

## 3. System Information and Monitoring

### System Information
```bash
# Display Linux system information
uname -a

# Show distribution info
cat /etc/os-release

# Display CPU information
lscpu

# Show memory usage
free -h

# Display disk usage
df -h

# Show information about block devices
lsblk
```

### Process Management
```bash
# Display dynamic real-time view of running processes
top

# Display a tree of processes
pstree

# List all processes
ps aux

# Kill a process by PID
kill 1234

# Kill a process by name
pkill process_name
```

## 4. Networking

### Network Configuration
```bash
# Show IP addresses and network interfaces
ip addr show

# Display routing table
ip route show

# Configure a network interface
sudo ip addr add 192.168.1.100/24 dev eth0

# Bring a network interface up or down
sudo ip link set eth0 up
sudo ip link set eth0 down
```

### Network Diagnostics
```bash
# Test network connectivity
ping google.com

# Trace route to host
traceroute google.com

# Show network connections and listening ports
netstat -tuln

# Capture and display network packets
sudo tcpdump -i eth0
```

## 5. User and Permission Management

### User Management
```bash
# Add a new user
sudo adduser username

# Delete a user
sudo deluser username

# Change user password
sudo passwd username

# Switch to another user
su - username

# Run command as root
sudo command
```

### File Permissions
```bash
# Change file permissions
chmod 755 file.txt

# Change file ownership
chown user:group file.txt

# Change ownership recursively
chown -R user:group directory
```

## 6. Package Management (Ubuntu/Debian)

```bash
# Update package lists
sudo apt update

# Upgrade installed packages
sudo apt upgrade

# Install a package
sudo apt install package_name

# Remove a package
sudo apt remove package_name

# Search for a package
apt search keyword

# Show package information
apt show package_name
```

## 7. Service Management with systemd

```bash
# Start a service
sudo systemctl start service_name

# Stop a service
sudo systemctl stop service_name

# Restart a service
sudo systemctl restart service_name

# Enable a service to start on boot
sudo systemctl enable service_name

# Disable a service from starting on boot
sudo systemctl disable service_name

# Check the status of a service
systemctl status service_name
```

## 8. Log Management

```bash
# View system logs
journalctl

# View logs for a specific service
journalctl -u service_name

# View logs since last boot
journalctl -b

# Follow logs in real-time
journalctl -f

# View logs within a time range
journalctl --since "2023-01-01" --until "2023-01-31"
```

## 9. Backup and Recovery

```bash
# Create a tar archive
tar -cvf backup.tar /path/to/directory

# Extract a tar archive
tar -xvf backup.tar

# Create a compressed tar archive (gzip)
tar -czvf backup.tar.gz /path/to/directory

# Synchronize directories (local or remote)
rsync -avz /source/directory/ /destination/directory/

# Create a disk image
sudo dd if=/dev/sda of=/path/to/disk.img bs=4M status=progress
```

## 10. Scripting Basics

```bash
#!/bin/bash

# Variables
NAME="John"
echo "Hello, $NAME!"

# Conditional statements
if [ "$NAME" == "John" ]; then
    echo "Name is John"
else
    echo "Name is not John"
fi

# Loops
for i in {1..5}; do
    echo "Number: $i"
done

# Functions
greet() {
    echo "Hello, $1!"
}

greet "Alice"
```

Remember to always exercise caution when using commands that can modify system settings or delete data, especially when using sudo or logged in as root. It's a good practice to test commands in a safe environment before applying them to production systems.
