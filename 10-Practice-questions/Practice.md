# Linux Scripting Solutions for DevOps Automation


## LEVEL 1: Linux + Bash Foundations


### 1️. Check file exists, print size & permissions or create file

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

FILE="example.txt"

if [ -f "$FILE" ]; then
    echo "File exists"
    ls -lh "$FILE" | awk '{print "Size:", $5, "Permissions:", $1}'
else
    echo "File not found, creating..."
    touch "$FILE"
fi
```

</details>

---

### 2️. Count lines, words, characters from argument file

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

FILE=$1

[ ! -f "$FILE" ] && echo "File not found" && exit 1

wc "$FILE"
```

</details>

---

### 3️. Compress `.log` files older than 7 days

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

find /var/log -name "*.log" -type f -mtime +7 -exec gzip {} \;
```

</details>

---

### 4️. Disk usage warning (>80%)

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

USAGE=$(df / | awk 'NR==2 {print $5}' | tr -d '%')

if [ "$USAGE" -gt 80 ]; then
    echo "WARNING: Disk usage is $USAGE%"
fi
```

</details>

---

### 5️. Save running processes to dated file

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

ps aux > processes_$(date +%F).txt
```

</details>

---

### 6️. Top 5 memory consuming processes

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

ps aux --sort=-%mem | head -6
```

</details>

---

### 7️. Print lines containing ERROR

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

while read -r line; do
    echo "$line" | grep "ERROR"
done < "$1"
```

</details>

---

### 8️. Check nginx service & start if not running

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

systemctl is-active --quiet nginx || systemctl start nginx
```

</details>

---

##  LEVEL 2: Scripting for Automation


### 9️ Monitor CPU every 5s for 1 minute

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

for i in {1..12}; do
    top -bn1 | grep "Cpu(s)"
    sleep 5
done
```

</details>

---

### 10. Log rotation (keep last 5)

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

mv app.log app.log.$(date +%Y%m%d)
touch app.log
ls -t app.log.* | tail -n +6 | xargs rm -f
```

</details>

---

### 1️1. Internet connectivity check

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

ping -c 1 google.com &>/dev/null || echo "$(date) Internet down" >> net.log
```

</details>

---

### 12. Create user if not exists

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

USER=$1
id "$USER" &>/dev/null || useradd -m -s /bin/bash "$USER"
```

</details>

---

### 1️3. Install package if not installed

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

PKG=curl
dpkg -l | grep -q "$PKG" || apt install -y "$PKG"
```

</details>

---

### 1️4. CSV parsing (col 1 & 3)

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

awk -F',' '{print $1, $3}' file.csv
```

</details>

---

### 1️5. APP_ENV based behavior

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

if [ "$APP_ENV" = "prod" ]; then
    set -euo pipefail
else
    echo "Non-production environment"
fi
```

</details>

---

## LEVEL 3: Real DevOps Scenarios

---

### 1️6. Web app monitoring & restart

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

STATUS=$(curl -s -o /dev/null -w "%{http_code}" http://localhost)

[ "$STATUS" -ne 200 ] && systemctl restart nginx
```

</details>

---

### 1️7. Backup `/etc` & cleanup old backups

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

mkdir -p /backup
tar czf /backup/etc_$(date +%F).tar.gz /etc
find /backup -type f -mtime +10 -delete
```

</details>

---

### 1️8. Export config file variables

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

while read -r line; do
    export "$line"
done < config.env
```

</details>

---

### 1️9. Docker container validation

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

CONTAINER=myapp

if [ "$(docker inspect -f '{{.State.Running}}' $CONTAINER)" != "true" ]; then
    docker restart $CONTAINER
    echo "$(date) Restarted $CONTAINER" >> docker.log
fi
```

</details>

---

### 2️0. Kubernetes CrashLoopBackOff pods

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

kubectl get pods --all-namespaces | grep CrashLoopBackOff
```

</details>

---

### 2️1. Stop on error + print line number

<details>
<summary> Solution</summary>

```bash
#!/bin/bash
set -e
trap 'echo "Error at line $LINENO"' ERR
```

</details>

---

### 2️2. Health check multiple servers

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

for server in s1 s2 s3; do
    ssh $server "uptime"
done
```

</details>

---

##  LEVEL 4: Advanced Automation

---

### 2️3. Retry logic

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

for i in {1..3}; do
    command && break || sleep 2
done
```

</details>

---

### 2️4. Single-instance lock

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

exec 200>/tmp/script.lock
flock -n 200 || exit 1
```

</details>

---

### 2️5. Generate `.env` from template

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

envsubst < template.env > .env
```

</details>

---

### 2️6. Auto-scale trigger on CPU

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

CPU=$(top -bn1 | awk '/Cpu/ {print 100-$8}')

[ "${CPU%.*}" -gt 80 ] && ./scale.sh
```

</details>

---

### 2️7. Capture logs & alert

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

if ! command > output.log 2>&1; then
    curl -X POST webhook_url -d "Job Failed"
fi
```

</details>

---

### 2️8. Validate CI prerequisites

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

command -v docker || echo "Docker missing"
command -v git || echo "Git missing"
ping -c 1 google.com || echo "Network down"
```

</details>

---

### 2️9. Clean unused Docker safely

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

docker system prune -f
```

</details>

---

### 3️0. Menu-driven DevOps CLI

<details>
<summary> Solution</summary>

```bash
#!/bin/bash

echo "1. Disk"
echo "2. Memory"
echo "3. Services"
read choice

case $choice in
1) df -h ;;
2) free -m ;;
3) systemctl list-units --type=service ;;
esac
```

</details>
