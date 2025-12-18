# Filesystem Basics

## 1. Introduction
The Linux filesystem is a hierarchical structure that organizes files and directories.
Understanding it is critical for DevOps engineers to manage servers, containers, and cloud VMs.

## 2. Key Commands
```bash
pwd
ls -la
cd /var/log
tree /
stat file.txt
chmod 755 script.sh
chown user:group file.txt
```

## 3. Ready-to-Use Practice Scripts
```bash
#!/bin/bash
echo "Listing root filesystem"
ls -l /
```

## 4. Hands-on Session
- Navigate to `/etc` and list config files
- Create directories under `/tmp`
- Change permissions and ownership

## 5. How This Helps in DevOps
- Debug container filesystems
- Manage config files
- Secure production servers