# Linux Advanced – Cheat Sheet

## 1. SSH (Secure Shell)

* Secure login and remote command execution

```bash
ssh user@host                      # Connect to remote host
ssh -p 22 user@host               # Connect using custom port
ssh-keygen -t rsa -b 4096         # Generate SSH key
ssh-copy-id user@host             # Copy public key to remote
scp file.txt user@host:/path/     # Copy file to remote
scp user@host:/path/file.txt .    # Copy file from remote
ssh -i key.pem user@host          # Connect using private key
```

---

## 2. Package Installer (APT – Debian/Ubuntu)

```bash
sudo apt update                   # Update package index
sudo apt upgrade                  # Upgrade installed packages
sudo apt install <package>        # Install a package
sudo apt remove <package>         # Remove a package
sudo apt autoremove               # Remove unused packages
dpkg -l | grep <package>          # List installed package
apt search <package>              # Search packages
apt show <package>                # Show package details
```

---

## 3. User Management

### User Commands

```bash
adduser username                  # Create a new user
userdel username                  # Delete a user
passwd username                   # Set/Change user password
usermod -aG sudo username         # Add user to sudo group
id username                       # Show user information
whoami                            # Show current user
su username                       # Switch to another user
sudo command                      # Run command as root
```

### User & Group Files

```text
/etc/passwd   → User account information
/etc/shadow   → Encrypted passwords
/etc/group    → Group information
/etc/sudoers  → Sudo privileges
```

### Permissions — rwx & Binary Table

| Permission    | Binary | Symbol | Meaning            |
| ------------- | ------ | ------ | ------------------ |
| Read          | 4      | r      | Read permission    |
| Write         | 2      | w      | Write permission   |
| Execute       | 1      | x      | Execute permission |
| No Permission | 0      | -      | No permission      |

### Permission Structure: `drwxrwxrwx`

* First character → File type
* Next 9 characters → Permissions (u,g,o)

| File Type | Meaning          |
| --------- | ---------------- |
| d         | Directory        |
| -         | Regular file     |
| l         | Symbolic link    |
| c         | Character device |
| b         | Block device     |
| p         | Named pipe       |

| Who       | r | w | x | Binary | Value     |
| --------- | - | - | - | ------ | --------- |
| User (u)  | r | w | x | 111    | 4+2+1 = 7 |
| Group (g) | r | w | x | 111    | 4+2+1 = 7 |
| Others(o) | r | w | x | 111    | 4+2+1 = 7 |

### Common Permission Examples

| Permission | Binary      | Meaning                                                     |
| ---------- | ----------- | ----------------------------------------------------------- |
| rwxrwxrwx  | 111 111 111 | 777 → Full access for all                                   |
| rwxr-xr-x  | 111 101 101 | 755 → User: full, Group: read+execute, Others: read+execute |
| rw-r--r--  | 110 100 100 | 644 → User: read+write, Group: read, Others: read           |
| rw-------  | 110 000 000 | 600 → User: read+write, Others: no access                   |

### Change Permissions

```bash
chmod <mode> <file>               # e.g., chmod 755 file.txt
chmod u+rwx,g+rx,o+r file.txt     # Symbolic method
chown user:group file.txt         # Change owner and group
chgrp group file.txt              # Change group
```

---

## 4. File Management

```bash
ls -l                             # Long listing
ls -lah                           # All files with human readable size
cd /path/to/dir                   # Change directory
pwd                               # Print working directory
cp src dest                       # Copy file or directory
cp -r dir1 dir2                   # Copy directory recursively
mv src dest                       # Move or rename
rm file                           # Remove file
rm -r dir                         # Remove directory recursively
rm -rf dir                        # Force remove (careful!)
touch file.txt                    # Create an empty file
cat file.txt                      # View file content
less file.txt                     # View file with pagination
head -n 10 file.txt               # First 10 lines
tail -n 10 file.txt               # Last 10 lines
echo "text" > file.txt           # Overwrite content
echo "text" >> file.txt          # Append content
ln -s target link_name            # Create symbolic link
find /path -name file.txt         # Find file
find / -type f -name "*.log"     # Find all .log files
grep "pattern" file.txt          # Search pattern in file
grep -r "pattern" /path          # Recursive search
du -sh *                          # Disk usage of files/dirs
df -h                             # Disk space usage
```

---

## 5. Process Management (systemctl, journalctl, etc.)

### Systemd (Service Management)

```bash
systemctl status <service>        # Check service status
systemctl start <service>         # Start a service
systemctl stop <service>          # Stop a service
systemctl restart <service>       # Restart a service
systemctl reload <service>        # Reload configuration
systemctl enable <service>        # Enable service on boot
systemctl disable <service>       # Disable service on boot
systemctl list-units --type=service  # List all services
```

### journalctl (Logs)

```bash
journalctl -u <service>           # Logs of a service
journalctl -xe                    # Recent logs with errors
journalctl -f                     # Follow logs live
journalctl --since "1 hour ago"  # Logs from last 1 hour
```

### Process Monitoring

```bash
ps aux                            # List running processes
top                               # Real-time process view
htop                              # Interactive process viewer
kill <PID>                        # Kill a process
kill -9 <PID>                     # Force kill a process
```

---

## 6. Volume Management (Disks & Partitions)

```bash
lsblk                             # List block devices
fdisk -l                          # List partitions
df -h                             # Disk space usage
du -sh /path                      # Folder size
mount /dev/sdb1 /mnt              # Mount device
umount /mnt                       # Unmount device
blkid                             # Show UUID, TYPE
mkfs.ext4 /dev/sdb1               # Format device (ext4)
```

---

## 7. Text Processing Commands

```bash
grep "text" file.txt             # Search text
awk '{print $1}' file.txt         # Print first column
awk -F: '{print $1,$3}' /etc/passwd  # Custom field separator
sed 's/old/new/g' file.txt        # Replace old with new
sed -n '1,10p' file.txt           # Print lines 1 to 10
find /path -name "*.conf"        # Find .conf files
```

---

## 5. Daily Must-Have Commands

```bash
ls -lah                           # List files
cd /path                          # Change directory
cat file                          # View file
grep "text" file                 # Search in file
systemctl status <service>        # Check service status
```

---

### Tip

Mastering these concepts will help you manage Linux systems like a pro!
