# Process and Service Management

## 1. Introduction
Processes and services are running programs on Linux.

## 2. Key Commands
```bash
ps aux
top
systemctl status nginx
systemctl restart nginx
kill -9 PID
```

## 3. Ready-to-Use Practice Scripts
```bash
#!/bin/bash
systemctl restart ssh
```

## 4. Hands-on Session
- Monitor CPU and memory
- Restart failed services

## 5. How This Helps in DevOps
- Troubleshooting outages
- Managing production services