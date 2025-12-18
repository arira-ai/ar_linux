Below is a **DevOps-oriented Linux scripting question set** designed to *force you to think like an automation engineer*, not just a Linux user.

These are **problem statements only**, progressing from **Beginner → Advanced**, and closely aligned with **real DevOps tasks** like monitoring, CI/CD, containers, and cloud automation.

# Linux Scripting Questions for DevOps Automation

## LEVEL 1: Linux + Bash Foundations (Must-Know)

1. Write a shell script to check whether a file exists.

   * If it exists, print file size and permissions
   * If not, create the file

2. Write a script that accepts a filename as an argument and counts:

   * Number of lines
   * Number of words
   * Number of characters

3. Write a script to find all `.log` files older than 7 days in `/var/log` and compress them.

4. Write a script that checks disk usage.

   * If usage exceeds **80%**, print a warning message

5. Write a script to list all running processes and save the output to a file with today’s date.

6. Write a script that prints the top **5 memory-consuming processes**.

7. Write a script that reads a file line by line and prints only lines containing the word `ERROR`.

8. Write a script to validate if a service (like `nginx`) is running.

   * If not running, start the service


## LEVEL 2: Scripting for Automation (DevOps Style)

9. Write a script to monitor CPU usage every 5 seconds for 1 minute.

10. Write a script to automatically rotate logs:

    * Rename `app.log` → `app.log.YYYYMMDD`
    * Keep only last **5 log files**

11. Write a script that checks internet connectivity.

    * If down, log the timestamp to a file

12. Write a script that takes a username as input:

    * Check if the user exists
    * If not, create the user and assign `/bin/bash`

13. Write a script to install a package only if it is **not already installed**.

14. Write a script that parses a CSV file and prints:

    * Column 1 and Column 3 only

15. Write a script that accepts an environment variable `APP_ENV`

    * If `prod`, enable strict mode
    * Else print “Non-production environment”


## LEVEL 3: Real DevOps Scenarios (Interview-Grade)

16. Write a script to monitor a web application:

    * Check HTTP status code
    * Restart service if status ≠ 200

17. Write a script that backs up `/etc` directory:

    * Archive it
    * Store with timestamp
    * Remove backups older than 10 days

18. Write a script that reads a config file:

    * Extract key=value pairs
    * Export them as environment variables

19. Write a script to validate a running Docker container.

    * If stopped, restart it
    * Log the action

20. Write a script that checks Kubernetes pod status using `kubectl`.

    * List only pods in `CrashLoopBackOff`

21. Write a script that runs a command and:

    * Stops execution immediately on failure
    * Prints the failed line number

22. Write a script to perform health checks on multiple servers using SSH.



## LEVEL 4: Advanced Automation (Production-Ready Thinking)

23. Write a script that implements **retry logic**:

    * Retry a command 3 times before failing

24. Write a script that:

    * Locks execution so only one instance runs at a time

25. Write a script that dynamically generates a `.env` file from a template.

26. Write a script to auto-scale:

    * Monitor CPU
    * Trigger another script if threshold exceeded

27. Write a script that:

    * Captures logs
    * Sends an alert (email / webhook) on failure

28. Write a script that validates CI pipeline prerequisites:

    * Docker
    * Git
    * Network
    * Required ports

29. Write a script that cleans unused Docker images and containers safely.

30. Write a script to create a **menu-driven CLI tool** for DevOps operations.
