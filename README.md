# 🐧 Linux for Cloud + DevOps — Beginner Guide

> A practical, command-driven guide to the Linux skills every Cloud/DevOps engineer needs — from distributions to disk management.

---

## 📖 Table of Contents

1. [Linux Distributions: Ubuntu & Amazon Linux](#1-linux-distributions-ubuntu-and-amazon-linux)
2. [Filesystem and Navigation](#2-filesystem-and-navigation)
3. [File and Directory Commands](#3-file-and-directory-commands)
4. [Text Commands](#4-text-commands)
5. [Pipes and Redirection](#5-pipes-and-redirection)
6. [Permissions: chmod, chown, Users & Groups](#6-permissions-chmod-chown-users-and-groups)
7. [Processes: ps, top, kill](#7-processes-ps-top-kill)
8. [Services and systemd](#8-services-and-systemd)
9. [Package Managers: apt and yum/dnf](#9-package-managers-apt-and-yumdnf)
10. [SSH](#10-ssh)
11. [Environment Variables](#11-environment-variables)
12. [Logs](#12-logs)
13. [Cron Jobs](#13-cron-jobs)
14. [Disk and Storage Commands](#14-disk-and-storage-commands)

---

## 1. Linux Distributions: Ubuntu and Amazon Linux

A **Linux distribution** = Linux + a package manager + default tools + configuration choices.

| Distribution   | Common Use                        | Package Manager |
|----------------|-------------------------------------|------------------|
| **Ubuntu**     | Learning, servers, development      | `apt`            |
| **Amazon Linux** | AWS-focused, built for EC2        | `dnf` (sometimes `yum`) |

**Identify your distribution:**

```bash
cat /etc/os-release
```

**Package command difference:**

```bash
# Ubuntu
sudo apt update
sudo apt install nginx

# Amazon Linux
sudo dnf install nginx
```

---

## 2. Filesystem and Navigation

Linux uses **one directory tree** starting at `/`.

| Path        | Purpose                     |
|-------------|-------------------------------|
| `/`         | Root of the filesystem        |
| `/home`     | User home folders             |
| `/etc`      | System configuration          |
| `/var/log`  | Log files                     |
| `/tmp`      | Temporary files                |
| `/usr/bin`  | Many installed commands       |

**Navigation commands:**

```bash
pwd                 # Show current directory
ls                   # List files/directories
ls -l                # Detailed list
ls -la               # Include hidden files
cd /var/log          # Move to an absolute path
cd ..                # Move up one directory
cd ~                 # Move to your home directory
cd -                 # Return to previous directory
```

**Example:**

```bash
cd /var/log
pwd
# /var/log

ls -l
```

---

## 3. File and Directory Commands

### `mkdir` — create directories
```bash
mkdir project
mkdir -p project/src/config   # Create nested directories
```

### `touch` — create an empty file or update its timestamp
```bash
touch notes.txt
```

### `cp` — copy files/directories
```bash
cp source.txt backup.txt
cp source.txt /tmp/
cp -r project project-backup   # Copy a directory recursively
```

### `mv` — move or rename
```bash
mv old-name.txt new-name.txt      # Rename
mv report.txt /home/user/docs/    # Move
```

### `rm` — remove files/directories
```bash
rm unwanted.txt
rm -r old-folder
rm -i important.txt   # Ask before removal
```

> ⚠️ **Be careful with recursive deletion:**
> ```bash
> rm -rf folder
> ```
> This **permanently** removes `folder` and its contents **without confirmation**.

---

## 4. Text Commands

Assume a file named `app.log`.

### `cat` — print an entire file
```bash
cat app.log
```
Good for small files.

### `less` — read large files one page at a time
```bash
less app.log
```

| Key     | Action          |
|---------|------------------|
| `Space` | Next page        |
| `b`     | Previous page    |
| `/word` | Search           |
| `q`     | Quit             |

### `head` and `tail`
```bash
head app.log         # First 10 lines
head -n 20 app.log    # First 20 lines

tail app.log          # Last 10 lines
tail -n 50 app.log     # Last 50 lines
tail -f app.log        # Keep watching new log entries
```

### `grep` — search text
```bash
grep "ERROR" app.log
grep -i "error" app.log          # Case-insensitive
grep -n "ERROR" app.log          # Show line numbers
grep -r "database" /etc          # Search recursively
grep -v "DEBUG" app.log          # Show lines NOT matching DEBUG
```

### `find` — locate files
```bash
find /var/log -name "*.log"
find . -type f -name "*.txt"
find . -type d -name "config"
find /tmp -mtime +7              # Modified more than 7 days ago
```

### `sort`, `uniq`, and `wc`
```bash
sort names.txt
sort names.txt | uniq            # Remove adjacent duplicate lines
sort names.txt | uniq -c         # Count each unique line

wc report.txt                    # Lines, words, bytes
wc -l report.txt                 # Only lines
```

**Example — find the most frequent IP addresses in a log:**
```bash
awk '{print $1}' access.log | sort | uniq -c | sort -nr
```

---

## 5. Pipes and Redirection

A **pipe** (`|`) sends the output of one command into another command.

```bash
grep "ERROR" app.log | wc -l
# Counts error lines
```

**Redirection** sends output to files.

```bash
ls -la > files.txt        # Overwrite files.txt
ls -la >> files.txt       # Append to files.txt
grep "ERROR" app.log 2> errors.txt  # Send error messages to errors.txt
```

**Common streams:**

| Stream  | Name   | Number |
|---------|--------|:------:|
| stdin   | input  | 0      |
| stdout  | normal output | 1 |
| stderr  | error output | 2 |

**Capture both normal output and errors:**
```bash
command > output.txt 2>&1
```

---

## 6. Permissions: chmod, chown, Users and Groups

Every file has:
- an **owner**
- a **group**
- **permissions** for owner, group, and everyone else

**View permissions:**
```bash
ls -l
```

**Example:**
```
-rwxr-xr-- 1 alice developers 1200 Aug 24 deploy.sh
```

| Who    | Permission | Meaning              |
|--------|------------|------------------------|
| Owner  | `rwx`      | read, write, execute   |
| Group  | `r-x`      | read, execute          |
| Others | `r--`      | read                    |

### `chmod` — change permissions
```bash
chmod +x script.sh        # Make executable
chmod 644 config.txt      # Owner read/write; others read
chmod 755 deploy.sh       # Owner full access; others read/execute
chmod 600 private.key     # Only owner can read/write
```

**Permission numbers:**

| Value | Meaning |
|:-----:|---------|
| 4     | read    |
| 2     | write   |
| 1     | execute |
| 7     | rwx     |
| 6     | rw-     |
| 5     | r-x     |

### `chown` — change owner/group
```bash
sudo chown alice file.txt
sudo chown alice:developers file.txt
sudo chown -R www-data:www-data /var/www/site
```

### Users and groups
```bash
whoami                       # Current user
id                            # User ID and group memberships
groups                        # Your groups
sudo useradd bob              # Create user
sudo passwd bob                # Set password
sudo usermod -aG sudo bob      # Ubuntu: add user to sudo group
```

On Amazon Linux, administrative users are often added to the `wheel` group instead:
```bash
sudo usermod -aG wheel bob
```

---

## 7. Processes: ps, top, kill

A **process** is a running program.

### View processes
```bash
ps
ps aux                   # Detailed list of all processes
ps aux | grep nginx
```

### Monitor live system activity
```bash
top
```
If installed, `htop` is a friendlier alternative:
```bash
htop
```

### Stop a process

Find its process ID (PID):
```bash
ps aux | grep myapp
```

Then stop it:
```bash
kill 1234        # Ask process to stop gracefully
kill -9 1234      # Force stop; use only if needed
```

Find and stop by name:
```bash
pkill nginx
```

---

## 8. Services and systemd

Many Linux services are managed by **systemd**.

**Check a service:**
```bash
sudo systemctl status nginx
```

**Start, stop, and restart:**
```bash
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
```

**Enable at system boot:**
```bash
sudo systemctl enable nginx
sudo systemctl disable nginx
```

**View all failed services:**
```bash
systemctl --failed
```

**Read its logs:**
```bash
sudo journalctl -u nginx
sudo journalctl -u nginx -f
```

---

## 9. Package Managers: apt and yum/dnf

A **package manager** installs and updates software.

### Ubuntu: `apt`
```bash
sudo apt update                 # Refresh package list
sudo apt upgrade                # Upgrade installed software
sudo apt install curl nginx
sudo apt remove nginx
sudo apt search docker
```

### Amazon Linux: `dnf`
```bash
sudo dnf check-update
sudo dnf upgrade
sudo dnf install curl nginx
sudo dnf remove nginx
sudo dnf search docker
```

Some older systems use `yum`; its commands are very similar:
```bash
sudo yum install nginx
```

---

## 10. SSH

SSH lets you securely connect to another server.

```bash
ssh username@server-ip
```

**Example:**
```bash
ssh ec2-user@54.123.45.67
```

**For an AWS EC2 private key:**
```bash
chmod 400 my-key.pem
ssh -i my-key.pem ec2-user@54.123.45.67
```

**Common default usernames:**

| Server Type       | Default User |
|--------------------|--------------|
| Ubuntu EC2          | `ubuntu`     |
| Amazon Linux EC2    | `ec2-user`   |

**Copy a file to a server:**
```bash
scp -i my-key.pem app.zip ec2-user@54.123.45.67:/home/ec2-user/
```

**Copy from a server:**
```bash
scp -i my-key.pem ec2-user@54.123.45.67:/home/ec2-user/app.log .
```

---

## 11. Environment Variables

Environment variables store values available to programs and shell sessions.

```bash
echo $HOME
echo $PATH
echo $USER
```

**Set one for the current terminal session:**
```bash
export APP_ENV=production
echo $APP_ENV
```

**Use it:**
```bash
mkdir "$HOME/my-project"
```

**Make variables persist** by adding them to `~/.bashrc` or `~/.zshrc`:
```bash
export APP_ENV=development
export PATH="$PATH:/opt/mytool/bin"
```

Then reload:
```bash
source ~/.bashrc
```

> ⚠️ Avoid putting passwords or API keys directly into shell history or shared scripts.

---

## 12. Logs

Logs record system and application events.

**Common locations:**

| Path                       | Purpose                              |
|------------------------------|----------------------------------------|
| `/var/log/syslog`            | Ubuntu system log                     |
| `/var/log/auth.log`          | Ubuntu authentication log             |
| `/var/log/messages`          | Amazon Linux / RHEL-style system log  |
| `/var/log/secure`            | Amazon Linux authentication log       |
| `/var/log/nginx/`            | Nginx logs                             |

**Useful commands:**
```bash
sudo tail -f /var/log/syslog
sudo grep "Failed password" /var/log/auth.log
sudo less /var/log/nginx/error.log
```

**For services managed by systemd:**
```bash
sudo journalctl -u ssh
sudo journalctl -u nginx --since "1 hour ago"
sudo journalctl -p err -b
```
The last command shows error-level messages from the current boot.

---

## 13. Cron Jobs

Cron schedules commands to run automatically.

**Edit your user's cron jobs:**
```bash
crontab -e
```

**List them:**
```bash
crontab -l
```

**Cron format:**
```
minute hour day-of-month month day-of-week command
```

**Examples:**
```bash
# Run every day at 2:30 AM
30 2 * * * /home/user/backup.sh

# Run every 5 minutes
*/5 * * * * /home/user/check-health.sh

# Run every Monday at 9 AM
0 9 * * 1 /home/user/weekly-report.sh
```

**Write output and errors to a log:**
```bash
0 2 * * * /home/user/backup.sh >> /home/user/backup.log 2>&1
```

> 💡 Use **full paths** in cron commands — cron has a limited environment.

---

## 14. Disk and Storage Commands

### Check free space
```bash
df -h
```
Output columns include filesystem, total size, used space, available space, and mount point.

### See directory sizes
```bash
du -sh /var/log
du -sh *
```

**Find largest items in a directory:**
```bash
du -sh /var/* 2>/dev/null | sort -h
```

### View disks and partitions
```bash
lsblk
```

**Show filesystem types and UUIDs:**
```bash
sudo blkid
```

### Check mounted filesystems
```bash
mount
```

### Mount a disk
```bash
sudo mkdir -p /mnt/data
sudo mount /dev/xvdf1 /mnt/data
```

**To keep a disk mounted after reboot**, add it to `/etc/fstab` — preferably using its UUID:
```bash
sudo blkid /dev/xvdf1
```

**Example `/etc/fstab` entry:**
```
UUID=your-uuid-here /mnt/data ext4 defaults,nofail 0 2
```

**Test `/etc/fstab` safely after editing:**
```bash
sudo mount -a
```
If that produces no errors, the configuration is usually valid.

---

*📌 Notes compiled for personal study — Linux fundamentals for Cloud/DevOps.*