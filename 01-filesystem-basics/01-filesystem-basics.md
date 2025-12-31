# Filesystem Basics: Essential Linux Guide


## 1. What is a Filesystem?
A **Filesystem** is the OS's internal logic for organizing data on a storage drive. Understanding it is critical for DevOps engineers to manage servers, containers, and cloud VMs.
* **Structure:** It turns raw blocks of data into a readable hierarchy of files and folders.
* **Metadata:** It tracks "data about data," such as file size, ownership, and creation dates.
* **Access Control:** It enforces security by defining who can read, write, or execute specific files.

##  2. Core Directory Reference

| Path | Purpose |
| :--- | :--- |
| `/` | **Root:** The starting point of the entire system. |
| `/bin` | **Binaries:** Essential user command executable files. |
| `/etc` | **Etcetera:** System-wide configuration files. |
| `/home` | **Home:** Personal storage for users (e.g., `/home/username`). |
| `/root` | **Root Home:** The private home directory for the Admin user. |
| `/tmp` | **Temporary:** Short-term files, usually wiped on reboot. |
| `/usr` | **User:** Static data for programs and libraries. |
| `/var` | **Variable:** Files that grow over time, like logs (`/var/log`). |
| `/dev` | **Devices:** Hardware interfaces (disks, terminals). |


## 3. Essential Commands

### Navigation & Management

1. `cd` : change directory                      
2. `pwd` : print working directory
3. `find` : used to search for files and dir. based on name, size, type, and modification time
4. `ls -la` : listing the files with help of flags output may vary
5. `stat file.txt` : fetch metadata of the file
6. `chmod 755 script.sh` : change the permission of the file
7. `chown user:group file.txt` : change the owner of the file


## 4. Hands-On: Permissions & Scripting

### The Task
1. Create a script named `hello.sh`.
2. Attempt to run it without execution permissions.
3. Grant permissions and run it again.

### Step 1: Create the Script
```bash
echo 'echo "Hello, Linux!"' > hello.sh

```

### Step 2: Execution Without Permissions

If you try to run the script immediately:

```bash
./hello.sh

```

**Response:**

> `bash: ./hello.sh: Permission denied`

### Step 3: Grant Permissions and Run

```bash
# Give the owner (u) execute (x) permissions
chmod u+x hello.sh

# Run again
./hello.sh

```

**Response:**

> `Hello, Linux!`


##  Challenges

### Medium Level : File Investigation

**Task:** Find the location of the `ls` binary and identify your current path.

* **Goal:** Use `which` and `pwd`.

<details>
<summary>View Solution</summary>

```bash
pwd        # Shows current working directory
which ls   # Likely returns /usr/bin/ls

```

</details>

### Hard Level: Advanced Search & Sort

**Task:** Find all `.conf` files in the `/etc` directory and save a list of their names to a file in your home directory.

* **Goal:** Practice `find` and output redirection.

<details>
<summary> View Solution</summary>

```bash
find /etc -name "*.conf" > ~/config_list.txt

```

</details>


## 5. How This Helps in DevOps
- Debug container filesystems
- Manage config files
- Secure production servers
