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

## 2. File Permissions and Ownership

### Viewing and Understanding Permissions
```bash
# View file permissions
ls -l

# Example output:
# -rwxr-xr-x 1 user group 4096 Jan 1 12:00 filename
```

- First character: File type (- for regular file, d for directory)
- Next 9 characters: Permissions for owner, group, and others
- Numeric representation: r (read) = 4, w (write) = 2, x (execute) = 1

### Changing Permissions
```bash
# Change permissions using numeric representation
chmod 755 filename  # rwxr-xr-x

# Add execute permission for all
chmod +x filename

# Remove write permission for group and others
chmod go-w filename
```

### Changing Ownership
```bash
# Change file owner
sudo chown newuser filename

# Change file group
sudo chgrp newgroup filename

# Change both owner and group
sudo chown newuser:newgroup filename
```

### Special Permissions
```bash
# Set SUID
chmod u+s filename

# Set SGID
chmod g+s filename

# Set Sticky Bit
chmod +t directory
```

## 3. File Editing

### Vim

1. Opening a file:
   ```
   vim filename
   ```

2. Vim modes:
   - Normal mode: Default, for navigation and commands
   - Insert mode: For editing text (press 'i' to enter)
   - Visual mode: For selecting text (press 'v' to enter)
   - Command mode: For executing commands (press ':' to enter)

3. Basic commands (in normal mode):
   ```
   i    # Enter insert mode
   Esc  # Return to normal mode
   :w   # Save file
   :q   # Quit (if no changes)
   :wq  # Save and quit
   :q!  # Quit without saving
   dd   # Delete current line
   yy   # Copy current line
   p    # Paste
   /pattern  # Search for 'pattern'
   n    # Move to next search result
   ```

4. Navigation:
   ```
   h    # Move left
   j    # Move down
   k    # Move up
   l    # Move right
   0    # Move to beginning of line
   $    # Move to end of line
   gg   # Move to beginning of file
   G    # Move to end of file
   ```

### Nano

1. Opening a file:
   ```
   nano filename
   ```

2. Basic commands:
   ```
   Ctrl + G  # Display help
   Ctrl + X  # Exit (prompts to save if changes made)
   Ctrl + O  # Save file
   Ctrl + W  # Search for text
   Ctrl + K  # Cut current line
   Ctrl + U  # Paste cut text
   ```

3. Navigation:
   ```
   Arrow keys  # Move cursor
   Ctrl + A    # Move to beginning of line
   Ctrl + E    # Move to end of line
   Ctrl + V    # Move down one page
   Ctrl + Y    # Move up one page
   ```

## 4. User Management

```bash
# Add a new user
sudo adduser username

# Delete a user
sudo deluser username

# Change user password
sudo passwd username

# Add user to a group
sudo usermod -aG groupname username

# View groups a user belongs to
groups username

# Switch to another user
su - username

# Run command as root
sudo command
```

## 5. Process Management

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

# Run a process in the background
command &

# Bring a background process to the foreground
fg

# Adjust process priority (nice value from -20 to 19)
nice -n 10 command
```

## 6. System Information and Monitoring

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

# Display system uptime
uptime

# Show current resource usage
top
```

## 7. Networking

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

# Test network connectivity
ping google.com

# Trace route to host
traceroute google.com

# Show network connections and listening ports
netstat -tuln

# Capture and display network packets
sudo tcpdump -i eth0

# Configure the firewall
sudo ufw enable
sudo ufw allow 22
```

## 8. Package Management (Ubuntu/Debian)

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

# List installed packages
dpkg -l

# Install a .deb package file
sudo dpkg -i package.deb
```

## 9. Service Management with systemd

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

# View service logs
journalctl -u service_name
```

## 10. Log Management

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

# View specific log file
less /var/log/syslog

# Search for a pattern in logs
grep "error" /var/log/syslog
```

## 11. Backup and Recovery

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

# Backup MySQL database
mysqldump -u username -p database_name > backup.sql

# Restore MySQL database
mysql -u username -p database_name < backup.sql
```

Remember to always exercise caution when using commands that can modify system settings or delete data, especially when using sudo or logged in as root. It's a good practice to test commands in a safe environment before applying them to production systems.
