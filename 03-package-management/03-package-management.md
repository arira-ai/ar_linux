# Package Management

## 1. Introduction
Package managers install, update, and remove software.

## 2. Key Commands
```bash
apt update
apt install nginx -y
apt remove nginx
apt autoremove
```

## 3. Ready-to-Use Practice Scripts
```bash
#!/bin/bash
apt update && apt install curl -y
```

## 4. Hands-on Session
- Install and remove packages
- Identify unused packages

## 5. How This Helps in DevOps
- Provision servers
- Build Docker images