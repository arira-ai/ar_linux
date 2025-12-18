# User and Group Management

## 1. Introduction
User and group management controls access and security on Linux systems.

## 2. Key Commands
```bash
useradd devuser
passwd devuser
groupadd devops
usermod -aG devops devuser
id devuser
```

## 3. Ready-to-Use Practice Scripts
```bash
#!/bin/bash
useradd testuser
echo "testuser created"
```

## 4. Hands-on Session
- Create users and groups
- Assign sudo access
- Verify permissions

## 5. How This Helps in DevOps
- Secure CI/CD agents
- Access control for servers