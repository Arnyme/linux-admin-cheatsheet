**Linux System Administrator Cheatsheet for SAP BASIS Trainees**

**Key Concepts for SAP BASIS:**

*   **`<SID>adm` user:** The primary OS user for managing an SAP system. `<SID>` is your System ID (e.g., `PRD`, `QAS`, `DEV`). Most SAP-related OS commands should be run as this user. Example: `s4hadm`, `erpadm`.
*   **`sapsys` group:** The primary group for SAP software. The `<SID>adm` user belongs to this group.
*   **Environment Variables:** SAP relies heavily on environment variables set for the `<SID>adm` user (e.g., `PATH`, `LD_LIBRARY_PATH`, `DIR_LIBRARY`, `DB_HOST`). Always use `su - <SID>adm` (with the hyphen) to ensure the correct environment is loaded.
*   **File Systems:** Pay close attention to:
    *   `/usr/sap/<SID>`: SAP system instance-specific directories.
    *   `/sapmnt/<SID>`: Shared SAP directory (profiles, global executables).
    *   `/hana/shared` (for HANA): Shared HANA installation files.
    *   `/hana/data/<SID>`, `/hana/log/<SID>` (for HANA): Data and log volumes.
    *   Mount points for databases, logs, and application binaries.
*   **Log Files:** You'll spend a lot of time in log files. Key locations:
    *   SAP work processes: `/usr/sap/<SID>/<Instance_ID>/work` (e.g., `dev_w0`, `dev_disp`, `dev_ms`).
    *   System logs: `/var/log/messages` or `journalctl`.
    *   Database logs (varies by DB).

---

**I. Navigation & File System Basics**

| Command         | Description                                      | Example Usage             |
| :-------------- | :----------------------------------------------- | :------------------------ |
| `pwd`           | Print Working Directory (show current path)      | `pwd`                     |
| `cd <dir>`      | Change Directory                                 | `cd /usr/sap/PRD`         |
| `cd ..`         | Go up one directory                              | `cd ..`                   |
| `cd ~` or `cd`  | Go to home directory                             | `cd ~`                    |
| `cd -`          | Go to the previous directory                     | `cd -`                    |
| `ls`            | List directory contents                          | `ls`                      |
| `ls -l`         | Long listing (permissions, owner, size, date)    | `ls -l`                   |
| `ls -a`         | List all files (including hidden, starts with `.`) | `ls -a`                   |
| `ls -h`         | Human-readable sizes (with `-l`)                 | `ls -lh`                  |
| `ls -ltr`       | Long list, sorted by time (reverse, newest last) | `ls -ltr`                 |
| `mkdir <dir>`   | Create a new directory                           | `mkdir backup_logs`       |
| `rmdir <dir>`   | Remove an empty directory                        | `rmdir old_logs`          |
| `tree <dir>`    | Display directory structure as a tree (if installed) | `tree /sapmnt/PRD`        |

---

**II. File & Directory Management**

| Command                   | Description                                          | Example Usage                     |
| :------------------------ | :--------------------------------------------------- | :-------------------------------- |
| `cp <source> <dest>`      | Copy file or directory                               | `cp file1.txt /tmp/`            |
| `cp -r <src_dir> <dest_dir>`| Copy directory recursively                           | `cp -r /usr/sap/PRD/D00/work /backup/` |
| `mv <source> <dest>`      | Move or rename file/directory                        | `mv old_name.txt new_name.txt`    |
| `rm <file>`               | Remove/delete a file (Be CAREFUL!)                   | `rm temp_file.log`                |
| `rm -r <dir>`             | Remove directory and its contents (Be VERY CAREFUL!) | `rm -r old_backup_dir`            |
| `rm -f <file/dir>`        | Force removal without prompting (DANGEROUS!)         | `rm -f junk.txt`                  |
| `touch <file>`            | Create an empty file or update timestamp             | `touch new_config.cfg`            |
| `find <path> -name "<pattern>"` | Find files by name                                 | `find /usr/sap -name "*.log"`      |
| `find <path> -type f -mtime -7` | Find files modified in the last 7 days         | `find /var/log -type f -mtime -7` |
| `stat <file/dir>`         | Display file or file system status                   | `stat /usr/sap/PRD/D00/exe/disp+work` |

---

**III. Viewing & Editing Files**

| Command                 | Description                                               | Example Usage             |
| :---------------------- | :-------------------------------------------------------- | :------------------------ |
| `cat <file>`            | Display entire file content (for short files)             | `cat /etc/hosts`          |
| `less <file>`           | View file content page by page (q to quit, / to search)   | `less dev_w0`             |
| `more <file>`           | Similar to `less`, older version                         | `more some_log.txt`       |
| `head <file>`           | Display first 10 lines of a file                          | `head large_log.txt`      |
| `head -n 20 <file>`     | Display first 20 lines                                    | `head -n 20 dev_disp`     |
| `tail <file>`           | Display last 10 lines of a file                           | `tail system.log`         |
| `tail -n 20 <file>`     | Display last 20 lines                                     | `tail -n 20 dev_ms`       |
| `tail -f <file>`        | Follow file as it grows (live log monitoring)             | `tail -f dev_w5`          |
| `vi <file>` or `vim <file>` | Powerful text editor (steep learning curve)               | `vi profile.txt`          |
| `nano <file>`           | Simple, user-friendly text editor                       | `nano notes.txt`          |

**Basic `vi`/`vim` commands:**
*   `i` - Enter insert mode
*   `Esc` - Exit insert mode (back to command mode)
*   `:w` - Write (save)
*   `:q` - Quit
*   `:wq` - Write and quit
*   `:q!` - Quit without saving
*   `/text` - Search for "text" (n for next, N for previous)

---

**IV. Permissions & Ownership**

| Command                       | Description                                       | Example Usage                               |
| :---------------------------- | :------------------------------------------------ | :------------------------------------------ |
| `chmod <permissions> <file>`  | Change file permissions (e.g., `755`, `u+x`)      | `chmod 750 script.sh`                       |
| `chmod -R <perms> <dir>`      | Change permissions recursively                      | `chmod -R 755 /usr/sap/PRD/SYS/profile`     |
| `chown <user>:<group> <file>` | Change file owner and group                       | `chown prdadm:sapsys some_file.dat`         |
| `chown -R <user>:<group> <dir>`| Change owner/group recursively                    | `chown -R qasadm:sapsys /usr/sap/QAS`       |
| `chgrp <group> <file>`        | Change file group                                 | `chgrp sapsys data_file.txt`                |
| `umask`                       | Shows or sets the default file creation permissions | `umask` (to show), `umask 022` (to set)   |

*Permissions (Octal):* `r=4, w=2, x=1`. `7 (rwx), 6 (rw-), 5 (r-x), 4 (r--), 0 (---)`
*Positions:* `Owner | Group | Others` (e.g., `750` = `rwxr-x---`)

---

**V. User & Group Management**

| Command                 | Description                                   | Example Usage             |
| :---------------------- | :-------------------------------------------- | :------------------------ |
| `whoami`                | Display current user                          | `whoami`                  |
| `id <username>`         | Display user and group information            | `id prdadm`               |
| `su <username>`         | Switch User (environment NOT fully loaded)    | `su prdadm`               |
| `su - <username>`       | Switch User (full login, loads environment)   | `su - prdadm` (IMPORTANT for SAP) |
| `sudo <command>`        | Execute command as superuser (root)           | `sudo systemctl restart nfs` |
| `exit`                  | Exit current shell or `su` session            | `exit`                    |
| `passwd <username>`     | Change user password                          | `passwd sidadm`           |
| `useradd <username>`    | Add a new user (often needs `sudo`)           | `sudo useradd newdba`     |
| `usermod -aG <group> <user>` | Add user to a supplementary group      | `sudo usermod -aG sapsys newdba` |
| `userdel <username>`    | Delete a user (often needs `sudo`)            | `sudo userdel olduser`    |
| `groupadd <groupname>`  | Add a new group (often needs `sudo`)          | `sudo groupadd dbadmins`  |

---

**VI. Process Management**

| Command                 | Description                                       | Example Usage             |
| :---------------------- | :------------------------------------------------ | :------------------------ |
| `ps aux` or `ps -ef`    | List all running processes                        | `ps aux \| grep disp+work` |
| `top`                   | Display real-time system processes (q to quit)    | `top`                     |
| `htop`                  | Interactive process viewer (if installed, better than `top`) | `htop`                 |
| `kill <PID>`            | Terminate a process by Process ID                 | `kill 12345`              |
| `kill -9 <PID>`         | Force kill a process (use as last resort)         | `kill -9 12345`           |
| `killall <proc_name>`   | Kill processes by name                            | `killall dw.sap`          |
| `pgrep <proc_name>`     | Find Process ID by name                           | `pgrep disp+work`         |
| `pkill <proc_name>`     | Signal processes based on name                    | `pkill -9 old_script`     |
| `jobs`                  | List background jobs                              | `jobs`                    |
| `bg %<job_id>`          | Send a stopped job to background                  | `bg %1`                   |
| `fg %<job_id>`          | Bring a background job to foreground              | `fg %1`                   |
| `nohup <command> &`     | Run command immune to hangups, in background      | `nohup ./long_script.sh &` |

**SAP Process Context:**
*   SAP start/stop: `startsap`, `stopsap` (as `<SID>adm`).
*   Check SAP processes: `ps -ef | grep dw.sap` (dialog work processes), `ps -ef | grep ms.sap` (message server), `ps -ef | grep en.sap` (enqueue server).

---

**VII. System Information & Monitoring**

| Command             | Description                                       | Example Usage          |
| :------------------ | :------------------------------------------------ | :--------------------- |
| `df -h`             | Display disk space usage (human-readable)         | `df -h`                |
| `du -sh <dir/file>` | Display disk usage of directory/file (summary, human-readable) | `du -sh /usr/sap/PRD` |
| `free -m` or `free -g` | Display memory usage (MB or GB)                 | `free -m`              |
| `vmstat <interval> <count>` | Report virtual memory statistics              | `vmstat 1 5`           |
| `iostat <interval> <count>` | Report CPU and I/O statistics                 | `iostat 2 5`           |
| `uname -a`          | Print all system information                      | `uname -a`             |
| `hostname`          | Display system hostname                           | `hostname`             |
| `uptime`            | Show system uptime and load average               | `uptime`               |
| `lscpu`             | Display CPU architecture information              | `lscpu`                |
| `lsblk`             | List block devices (disks, partitions)            | `lsblk`                |
| `cat /etc/os-release` | Show OS distribution information                  | `cat /etc/os-release`  |
| `dmesg`             | Print or control the kernel ring buffer           | `dmesg \| tail`        |
| `journalctl -xe`    | View systemd journal logs with explanations       | `journalctl -xe`       |
| `journalctl -u <service>`| View logs for a specific systemd unit         | `journalctl -u nfs-server` |

---

**VIII. Networking**

| Command                     | Description                                      | Example Usage                 |
| :-------------------------- | :----------------------------------------------- | :---------------------------- |
| `ip addr` or `ifconfig`     | Display network interface configuration          | `ip addr show`                |
| `ping <host/IP>`            | Test network connectivity                        | `ping google.com`             |
| `netstat -tulnp`            | Show listening TCP/UDP ports and processes (may need `sudo`) | `netstat -tulnp`      |
| `ss -tulnp`                 | Newer replacement for `netstat` (may need `sudo`) | `ss -tulnp`                   |
| `traceroute <host/IP>`      | Trace network path to host                       | `traceroute dbserver`         |
| `nslookup <hostname/IP>`    | Query DNS servers                                | `nslookup sapappserver01`     |
| `dig <hostname>`            | More advanced DNS lookup utility                 | `dig @8.8.8.8 sap.com`        |
| `route -n` or `ip route`    | Display routing table                            | `ip route show`               |
| `scp <file> user@host:<path>` | Secure copy file to/from remote host           | `scp backup.tar.gz prdadm@remotesrv:/tmp/` |
| `ssh <user>@<host>`         | Secure Shell (remote login)                      | `ssh prdadm@sapapp01`         |

**SAP Port Context:** SAP uses many ports. Common ones:
*   Dispatcher: `32<Instance_No>` (e.g., 3200)
*   Message Server: `36<Instance_No>` (e.g., 3600)
*   Gateway: `33<Instance_No>` (e.g., 3300)
*   ICM HTTP/HTTPS: `80<Instance_No>`, `443<Instance_No>`

---

**IX. Searching & Text Manipulation**

| Command                          | Description                                       | Example Usage                                     |
| :------------------------------- | :------------------------------------------------ | :------------------------------------------------ |
| `grep "<pattern>" <file>`        | Search for pattern in file (case-sensitive)       | `grep "ERROR" dev_w0`                             |
| `grep -i "<pattern>" <file>`     | Case-insensitive search                           | `grep -i "connection refused" alert_SID.log`      |
| `grep -v "<pattern>" <file>`     | Invert match (show lines NOT matching)            | `grep -v "DEBUG" application.log`                 |
| `grep -r "<pattern>" <dir>`      | Recursive search in directory                     | `grep -r "ORA-" /usr/sap/PRD/D00/work`            |
| `command | grep "<pattern>"`     | Pipe command output to grep                       | `ps -ef | grep sapstartsrv`                        |
| `awk '{print $1, $3}' <file>`    | Pattern scanning and processing language          | `df -h | awk '{print $1, $5}'`                    |
| `sed 's/old/new/g' <file>`       | Stream editor for basic text transformations      | `sed 's/localhost/dbhost01/g' config.ini`         |
| `sort <file>`                    | Sort lines of text files                          | `sort unsorted_list.txt`                          |
| `uniq <file>`                    | Report or omit repeated lines (needs sorted input) | `sort access.log | uniq -c` (counts occurrences) |
| `wc -l <file>`                   | Count lines in a file                             | `wc -l large.log`                                 |
| `wc -w <file>`                   | Count words                                       | `wc -w document.txt`                              |
| `wc -c <file>`                   | Count bytes                                       | `wc -c data.bin`                                  |

---

**X. Archiving & Compression**

| Command                         | Description                                       | Example Usage                                     |
| :------------------------------ | :------------------------------------------------ | :------------------------------------------------ |
| `tar -cvf <archive.tar> <files/dirs>` | Create TAR archive                              | `tar -cvf logs_backup.tar /var/log/sap`           |
| `tar -xvf <archive.tar>`        | Extract TAR archive                               | `tar -xvf logs_backup.tar`                        |
| `tar -tvf <archive.tar>`        | List contents of TAR archive                      | `tar -tvf old_backup.tar`                         |
| `tar -czvf <archive.tar.gz> <f/d>`| Create gzipped TAR archive                        | `tar -czvf profiles.tar.gz /sapmnt/PRD/profile`   |
| `tar -xzvf <archive.tar.gz>`    | Extract gzipped TAR archive                       | `tar -xzvf profiles.tar.gz -C /tmp/restore`       |
| `gzip <file>`                   | Compress file (replaces original with `.gz`)      | `gzip large_trace.trc`                            |
| `gunzip <file.gz>`              | Decompress `.gz` file                             | `gunzip large_trace.trc.gz`                       |
| `zip <archive.zip> <files/dirs>`| Create ZIP archive                                | `zip -r backup.zip /usr/sap/config`               |
| `unzip <archive.zip>`           | Extract ZIP archive                               | `unzip backup.zip -d /mnt/recovery`               |

---

**XI. Package Management (Distro-Specific)**

| Distribution        | Update Repo | Install Package     | Remove Package      | Search Package     | List Installed |
| :------------------ | :---------- | :------------------ | :------------------ | :----------------- | :------------- |
| **RHEL/CentOS/Oracle Linux** (yum/dnf) | `sudo yum update` or `sudo dnf update` | `sudo yum install <pkg>` or `sudo dnf install <pkg>` | `sudo yum remove <pkg>` or `sudo dnf remove <pkg>` | `yum search <pkg>` or `dnf search <pkg>` | `yum list installed` or `dnf list installed` |
| **SLES/openSUSE** (zypper) | `sudo zypper ref` | `sudo zypper in <pkg>` | `sudo zypper rm <pkg>` | `zypper se <pkg>`  | `zypper pa -i` |
| **Debian/Ubuntu** (apt) | `sudo apt update` | `sudo apt install <pkg>` | `sudo apt remove <pkg>` | `apt search <pkg>` | `apt list --installed` |

*You'll often use this to install prerequisites for SAP or database installations.*

---

**XII. Services / Daemons (Systemd)**

(Most modern Linux distributions use `systemd`)

| Command                                 | Description                                 |
| :-------------------------------------- | :------------------------------------------ |
| `systemctl status <service_name>`       | Check service status                        |
| `systemctl start <service_name>`        | Start a service                             |
| `systemctl stop <service_name>`         | Stop a service                              |
| `systemctl restart <service_name>`      | Restart a service                           |
| `systemctl enable <service_name>`       | Enable service to start on boot             |
| `systemctl disable <service_name>`      | Disable service from starting on boot       |
| `systemctl is-active <service_name>`    | Check if service is active                  |
| `systemctl is-enabled <service_name>`   | Check if service is enabled on boot         |
| `systemctl list-units --type=service --all` | List all services                       |

*Examples: `sudo systemctl status nfs-server`, `sudo systemctl restart chronyd`*
*For older SysVinit systems: `service <service_name> status/start/stop/restart`*

---

**XIII. Important Tips for SAP BASIS Trainees**

1.  **ALWAYS `su - <SID>adm`:** The hyphen is critical. It ensures the correct SAP environment (paths, libraries) is loaded.
2.  **Know Your Directories:** `/usr/sap/<SID>`, `/sapmnt/<SID>`, instance work directories, profile directories are your bread and butter.
3.  **Log Files are Your Friends:** `dev_w*`, `dev_disp`, `dev_ms`, `dev_icm`, database alert logs, system messages. Learn to `tail -f` and `grep` them effectively.
4.  **Be Careful with `rm`:** Especially `rm -rf`. Double-check paths. There's no undelete.
5.  **Use `man <command>`:** The manual pages are your best friend for understanding command options (e.g., `man ls`, `man find`).
6.  **Tab Completion:** Press `Tab` to auto-complete commands and paths. Saves time and typos.
7.  **History:** Use `history` command or `Ctrl+R` to search command history.
8.  **Piping `|` and Redirection `>` `>>`:**
    *   `command1 | command2`: Sends output of `command1` as input to `command2`. (e.g., `ls -l | grep ".txt"`)
    *   `command > file.txt`: Redirects output to `file.txt` (overwrites).
    *   `command >> file.txt`: Appends output to `file.txt`.
9.  **Understand File System Full Scenarios:** `df -h` is your go-to. Know what to do if `/usr/sap` or database filesystems get full (often involves archiving old logs/traces or extending the FS).
10. **Permissions Matter:** SAP is sensitive to file ownership and permissions. Incorrect settings can stop SAP from starting or functioning. Typically, files under `/usr/sap/<SID>` and `/sapmnt/<SID>` should be owned by `<SID>adm:sapsys`.
11. **Backup Regularly:** While not directly a Linux command, ensure your Linux OS and SAP data are backed up. You might be involved in configuring backup scripts.
12. **Documentation:** Keep notes of common tasks, server names, SIDs, and any specific configurations for your landscape.

This cheatsheet should give you a strong foundation. As you work with SAP systems, you'll discover which commands you use most frequently. Good luck!
