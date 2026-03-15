# Linux Backup Automation (rsync + cron)

## Overview

This project demonstrates how to automate file backups in Linux using **rsync** and **cron** on **AlmaLinux**.
The system performs scheduled backups from a source directory to a backup directory and records the activity in a log file.

---

## Architecture

```
        +-------------+
        |  /data      |
        | Source Data |
        +------+------+
               |
               | rsync
               v
        +------+------+
        |  /backup    |
        | Backup Data |
        +------+------+
               |
               | Logging
               v
        +--------------+
        | /var/log     |
        | backup.log   |
        +--------------+

Automation: cron job runs the backup script daily
```

---

## Technologies Used

* AlmaLinux
* rsync
* cron
* Bash scripting
* GitHub

---

## Directory Structure

```
linux-backup-automation/
│
├── backup.sh
├── README.md
└── screenshots/
```

---

## Step 1: Install rsync

Check if rsync is installed:

```bash
rsync --version
```

Install if required:

```bash
dnf install rsync -y
```
![rsync version](screenshots/01-rsync-version.png)
---

## Step 2: Create Source Directory

```bash
mkdir /data
touch /data/file1.txt
touch /data/file2.txt
touch /data/file3.txt
```

Verify:

```bash
ls /data
```
![data-directory](screenshots/02-data-directory.png)
---

## Step 3: Create Backup Directory

```bash
mkdir /backup
```
![backup-directory](screenshots/03-backup-directory.png)
---

## Step 4: Perform Manual Backup

```bash
rsync -av /data/ /backup/
```

### Explanation

| Option | Meaning                                          |
| ------ | ------------------------------------------------ |
| -a     | archive mode (preserves permissions, timestamps) |
| -v     | verbose output                                   |

![manual-backup](screenshots/04-manual-backup.png)
---

## Step 5: Test Incremental Backup

Modify a file:

```bash
vim /data/file1.txt
```

Run rsync again:

```bash
rsync -av /data/ /backup/
```

Only the modified file will be synchronized.

![incremental-backup](screenshots/05-incremental-backup.png)
---

## Step 6: Create Backup Script

Create script:

```bash
vim /usr/local/bin/backup.sh
```

Script:

```bash
#!/bin/bash

SOURCE="/data/"
DEST="/backup/"
LOGFILE="/var/log/backup.log"

rsync -av $SOURCE $DEST >> $LOGFILE 2>&1
```

Make executable:

```bash
chmod +x /usr/local/bin/backup.sh
```

Test:

```bash
/usr/local/bin/backup.sh
```
![backup-script](screenshots/06-backup-script1.png)
![backup-script](screenshots/07-backup-script2.png)
---

## Step 7: Automate Backup with cron

Edit cron:

```bash
crontab -e
```

Add job:

```bash
0 2 * * * /usr/local/bin/backup.sh
```

Meaning:

| Time    | Value |
| ------- | ----- |
| Minute  | 0     |
| Hour    | 2     |
| Day     | *     |
| Month   | *     |
| Weekday | *     |

This runs the backup **daily at 2:00 AM**.

Check cron jobs:

```bash
crontab -l
```

---

## Step 8: Verify Logs

Check backup log:

```bash
cat /var/log/backup.log
```

---

## Screenshots

Screenshots are stored in the `screenshots` directory.

Examples include:

* rsync installation
* source directory creation
* backup execution
* backup script
* cron configuration
* backup logs

---

## Key Features

* Automated backup using cron
* Incremental synchronization with rsync
* Backup logging for monitoring
* Simple and efficient Linux backup solution

---

## Future Improvements

* Remote backups using rsync over SSH
* Email notification for backup failures
* Backup compression
* Multiple backup directories

---

## Author

Bibin Mathew  
RHCSA Certified Linux System Administrator

Created as part of Linux administration learning and portfolio development.
