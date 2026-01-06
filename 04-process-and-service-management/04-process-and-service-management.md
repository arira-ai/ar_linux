# Process and Service Management in Linux

## 1. Introduction: Why This Matters

Linux runs everything through **processes and services**.
Every web server, database, CI runner, cron job, and monitoring agent is ultimately a **Linux process**.

For DevOps engineers, mastering this topic means:

* Faster incident recovery
* Safe production troubleshooting
* Understanding **self-healing at OS level**
* Confidence in automation and scripting

Modern Linux systems use **systemd** as the init and service manager.


## 2. Core Linux Concepts

### Process

* A **running instance of a program**
* Identified by a **PID (Process ID)**
* Can be short-lived or long-running

### Daemon

* A **background process**
* Usually starts at boot
* Example: `sshd`, `nginx`, `cron`

### Service

* A daemon **managed by systemd**
* Can be started, stopped, restarted, monitored


## 3. Linux Boot → systemd → Services (Init System Flow)

```mermaid

flowchart LR
    A["1. Power On 
    (Hardware Check)"] --> B["2. Bootloader 
    (The Menu)"]
    B --> C["3. Kernel 
    (The Manager)"]
    C --> D["4. Systemd 
    (The Supervisor)"]
    D --> E["5. Targets 
    (The Goal)"]
    E --> F["6. Login Screen 
    (Ready!)"]

    style A fill:#000,color:#fff
    style F fill:#000,color:#fff
```

> **Key takeaway:**
> `systemd` is the **first user-space process** and controls everything.


## 4. Understanding systemd (The Service Manager)

systemd is responsible for:

* Starting services
* Restarting failed services
* Tracking service state
* Centralized logging (journal)

### systemd Components

| Component | Purpose                 |
| --------- | ----------------------- |
| Unit      | Service definition file |
| Target    | Group of services       |
| Journal   | Logging system          |
| Daemon    | Background service      |

---

## 5. systemctl: Controlling Services (Flow Diagram)

```mermaid
flowchart TD
    A[Linux Kernel Boot]
    A --> B[init system PID 1]
    B --> C[systemd]
    C --> D[Reads Unit Files]
    D --> E[Service Definition]
    F[systemctl command]
    F --> C

    C --> G[Start Service]
    C --> H[Stop Service]
    C --> I[Restart Service]
    C --> J[Monitor Service]

    G --> K[Daemon Process]
    H --> K
    I --> K
    J --> K

    K --> L[Runs in Background]
    L --> M[Consumes CPU / Memory]
```

```mermaid
graph TD
    %% Main Concept
    SD["systemd (PID 1)"]
    SD --- Role["System Supervisor"]
    
    %% Section 1: Core Responsibilities
    subgraph Tasks ["Core Functions"]
        T1["Boot Management"]
        T2["Process Tracking (Re-parenting)"]
        T3["Log Management (journald)"]
    end
    SD --> Tasks

    %% Section 2: Unit Types
    subgraph Units ["Common Unit Types (.unit)"]
        U1["<b>.service</b><br/>Applications/Daemons"]
        U2["<b>.target</b><br/>Groups (Runlevels)"]
        U3["<b>.mount</b><br/>Storage/Partitions"]
        U4["<b>.socket</b><br/>IPC/Networking"]
    end
    SD --> Units

    %% Section 3: File Locations & Priority
    subgraph Paths ["File Locations (Priority Low to High)"]
        P1["/lib/systemd/system/<br/>(OS Defaults)"]
        P2["/usr/lib/systemd/system/<br/>(Installed Apps)"]
        P3["/etc/systemd/system/<br/>(Custom Overrides)"]
    end
    Units --> Paths
    style P3 fill:#f9f,stroke:#333,stroke-width:2px

    %% Section 4: Anatomy of a Service
    subgraph Anatomy ["Service File Sections"]
        S1["<b>[Unit]</b><br/>Description & Metadata"]
        S2["<b>[Service]</b><br/>ExecStart & Process Type"]
        S3["<b>[Install]</b><br/>Boot Triggers (WantedBy)"]
    end
    P3 -.-> Anatomy

    %% Section 5: Commands
    subgraph CMD ["Common Commands"]
        C1["systemctl daemon-reload"]
        C2["systemctl restart"]
        C3["systemctl status"]
    end
    Anatomy --> CMD
```

---

## 6. Essential Commands (Daily Linux Ops)

### Process Monitoring

```bash
ps aux
top
htop
```

### Service Management

```bash
systemctl status nginx
systemctl start nginx
systemctl stop nginx
systemctl restart nginx
```

### Enable Service at Boot

```bash
systemctl enable nginx
```

---

## 7. Hands-On Lab 1: Process Monitoring (Start Here)

```bash
top
ps aux --sort=-%cpu | head
ps aux --sort=-%mem | head
```

### Tasks

* Identify CPU-heavy processes
* Identify memory-heavy processes
* Note PID and command name

---

## 8. Daemon Lifecycle (How Background Services Run)

```mermaid
flowchart TD
    A[Daemon Started]
    A --> B[Runs in Background]
    B --> C[Consumes Resources]
    C --> D{Failure?}
    D -->|Yes| E[Exit / Crash]
    D -->|No| B
    E --> F[systemd Detects]
    F --> G[Restart Policy Applied]
```


## 9. Hands-On Lab 2: Service Failure Simulation

```bash
systemctl stop nginx
systemctl status nginx
systemctl start nginx
```

> Observe service state transitions clearly.

---

## 10. journalctl: Centralized Logging Explained

```mermaid
flowchart TD
    A[Service Runs]
    A --> B[Logs Generated]
    B --> C[journald]
    C --> D[Stored in Journal]
    D --> E[journalctl Query]
```

### Common Log Commands

```bash
journalctl -u nginx
journalctl -xe
journalctl --since "10 minutes ago"
```

---

## 11. kill Command & Signal Handling (Critical Concept)

```mermaid
flowchart TD
    A[Admin / Script]
    A --> B[kill Signal]
    B --> C{Signal Type}
    C -->|SIGTERM| D[Graceful Shutdown]
    C -->|SIGKILL| E[Force Kill]
    D --> F[Process Cleanup]
    E --> G[Immediate Termination]
```

### Commands

```bash
kill PID
kill -15 PID   # graceful
kill -9 PID    # force (dangerous)
```

---

## 12. Hands-On Lab 3: Kill & Recover a Process

```bash
pidof nginx
kill -9 <PID>
systemctl status nginx
```

> If configured, **systemd restarts the service automatically**.

---

## 13. Zombie & Orphan Processes (Linux Internals)

### Definitions

* **Zombie:** Process finished, parent didn’t collect status
* **Orphan:** Parent died, child still running

### Identify Zombies

```bash
ps aux | awk '$8 ~ /Z/'
```

### Identify Orphans

```bash
ps -eo pid,ppid,cmd | awk '$2==1'
```

> Orphans are adopted by **PID 1 (systemd)**

---

## 14. systemd Unit File (Service Definition)

A systemd unit file defines what a service is, how it should run, and what systemd should do if it fails.

### Location

```bash
/etc/systemd/system/
```

### Example

```ini
[Unit]
Description=My App Service
After=network.target

[Service]
ExecStart=/usr/bin/python3 /opt/app/app.py
Restart=always
RestartSec=5
User=appuser

[Install]
WantedBy=multi-user.target
```

### Apply Changes

```bash
systemctl daemon-reload
systemctl start myapp
systemctl enable myapp
```

---

## 15. Self-Healing at Linux Level (Auto-Restart)

```mermaid
flowchart TD
    A[Service Running]
    A --> B[Process Crash]
    B --> C[systemd Detects Failure]
    C --> D[Restart Policy]
    D --> E[Service Restarted]
    E --> A
```

> This is **Linux-native self-healing**

---

## 16. Common Production Mistakes

* Overusing `kill -9`
* Restarting without checking logs
* Running services as root
* Missing restart policies
* Ignoring resource usage

---

## 17. Real-World Troubleshooting Flow

**Problem:** Application unresponsive

```bash
top
ps aux --sort=-%cpu
journalctl -u app
kill PID
systemctl restart app
```

---

## 18. Why Linux Skills Matter (Beginner Motivation)

Linux is the **foundation of all modern technology**:

* Cloud servers
* Docker containers
* CI/CD runners
* Databases
* Monitoring systems

If you understand **processes, services, logs, and signals**, you can troubleshoot **any stack** with confidence.

> **Master Linux → Everything else becomes easier**

---

## 19. Interview Rapid-Fire Commands

```bash
ps aux
top
systemctl status
journalctl -xe
kill -9 PID
systemctl daemon-reload
```

---

### One-Line DevOps Summary

> **systemd + processes + logs = Linux self-healing engine**
