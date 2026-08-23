# Complete Linux Guide for Cloud Computing Beginners

> Beginner-friendly Linux notes for managing cloud servers, applications, logs, networking, permissions, and automation.

## 1. Why Linux for Cloud Computing?

Most AWS EC2, Azure VM, and Google Compute Engine servers run Linux. Knowing the command line helps you connect to servers, deploy applications, troubleshoot logs, and automate repeatable work.

## 2. Linux filesystem hierarchy

| Directory | Purpose |
| --- | --- |
| `/` | Root of the entire filesystem |
| `/home` | Regular users' home folders |
| `/etc` | System configuration files |
| `/var` | Variable data such as logs and caches |
| `/var/log` | System and application logs |
| `/bin`, `/usr/bin` | Essential command programs |
| `/sbin` | System administration programs |
| `/tmp` | Temporary files |
| `/opt` | Optional or third-party software |
| `/root` | Root user's home folder |
| `/dev` | Device files |
| `/proc` | Virtual information about processes and the kernel |

## 3. Navigation

```bash
pwd                    # Print current directory
ls                     # List files
ls -l                  # List with details
ls -la                 # Include hidden files
ls -lh                 # Human-readable file sizes
cd /var/log            # Change to this directory
cd ..                  # Move up one directory
cd ~                   # Go to your home directory
cd -                   # Return to the previous directory
tree                   # Show a directory tree (may need installation)
```

Example:

```bash
cd /home/ec2-user/projects
ls -la
```

## 4. File and directory operations

```bash
touch file.txt                 # Create an empty file
mkdir myfolder                 # Create a directory
mkdir -p a/b/c                 # Create nested directories
cp file.txt backup.txt         # Copy a file
cp -r folder1 folder2          # Copy a directory and its contents
mv file.txt newname.txt        # Rename a file
mv file.txt /tmp/              # Move a file
rm -i file.txt                 # Delete with confirmation
rm -r folder                   # Delete a directory and its contents
find / -name "*.log" 2>/dev/null  # Find log files
locate filename                # Fast search (database may need updating)
```

> **Warning:** `rm -rf folder` deletes without confirmation. It usually cannot be undone.

Example deployment folder:

```bash
mkdir -p /home/ec2-user/app
cp -r ./mywebapp/. /home/ec2-user/app/
```

## 5. Viewing and editing files

```bash
cat file.txt                   # Print a small file
less file.txt                  # Read page by page; press q to exit
more file.txt                  # Older pager
head -n 20 file.txt            # First 20 lines
tail -n 20 file.txt            # Last 20 lines
tail -f /var/log/syslog        # Follow new log lines live
nano file.txt                  # Simple terminal editor
vim file.txt                   # Advanced terminal editor
```

Example:

```bash
tail -f /var/log/nginx/error.log
```

Press `Control + C` to stop following a log.

## 6. Permissions and ownership

Linux permissions are read (`r`), write (`w`), and execute (`x`) for the owner, group, and others.

```bash
ls -l file.txt
chmod 755 script.sh             # Owner: rwx; group/others: r-x
chmod +x script.sh              # Add execute permission
chmod -w file.txt               # Remove write permission
sudo chown user:group file.txt  # Change owner and group
sudo chown -R user:group folder # Change ownership recursively
```

| Number | Permission |
| --- | --- |
| `7` | `rwx` |
| `6` | `rw-` |
| `5` | `r-x` |
| `4` | `r--` |
| `0` | no permission |

Example:

```bash
chmod +x deploy.sh
./deploy.sh
```

## 7. User and group management

```bash
whoami                       # Current user
id                           # User and group IDs
groups                       # Current user's groups
sudo useradd john            # Create a user
sudo passwd john             # Set a password
sudo usermod -aG sudo john   # Add a user to the sudo group
sudo deluser john            # Delete a user (Debian/Ubuntu)
su - john                    # Switch to user john
sudo command                 # Run a command as administrator
```

## 8. Process management

```bash
ps                           # Current shell processes
ps aux                       # All running processes
top                          # Live CPU and memory view; press q to quit
htop                         # Friendlier interactive view (may need installation)
kill 1234                    # Stop process with PID 1234
kill -9 1234                 # Force-stop process; use carefully
killall firefox              # Stop processes by name
jobs                         # Background jobs in this shell
bg                           # Continue a stopped job in background
fg                           # Bring a job to foreground
nohup command &              # Run a command in background after logout
```

Example: find what uses port 8080, then stop it.

```bash
sudo lsof -i :8080
kill <PID>
```

## 9. Disk and system information

```bash
df -h                         # Disk space
du -sh folder/                # Size of a folder
free -h                       # Memory usage
uname -a                      # Kernel and system information
uptime                        # Running time and system load
lscpu                         # CPU information
history                       # Command history
clear                         # Clear the terminal
```

## 10. Package management

### Debian and Ubuntu (APT)

```bash
sudo apt update
sudo apt upgrade
sudo apt install nginx
sudo apt remove nginx
```

### RHEL, CentOS, and Amazon Linux (YUM/DNF)

```bash
sudo yum update
sudo yum install httpd
sudo dnf install nginx
```

## 11. Networking and remote access

```bash
ip a                             # Network interfaces and IP addresses
ping google.com                  # Test connectivity; Control + C stops it
curl https://example.com         # Request a URL
wget https://example.com/file.zip # Download a file
ss -tulpn                        # Listening ports (modern replacement for netstat)
ssh user@remote_ip               # Connect over SSH
scp file.txt user@remote:/path/  # Copy file to a remote server
```

Example AWS connection:

```bash
ssh -i mykey.pem ec2-user@<public-ip>
```

## 12. Text processing and searching

```bash
grep "error" logfile.txt             # Find matching lines
grep -r "TODO" ./src/                # Search a directory recursively
grep -i "error" logfile.txt          # Ignore letter case
wc -l file.txt                       # Count lines
sort file.txt                        # Sort lines
uniq file.txt                        # Remove adjacent duplicate lines
cut -d',' -f1 data.csv               # First CSV column
awk '{print $1}' file.txt            # First field of each line
sed 's/old/new/g' file.txt           # Print with replacement
```

Example:

```bash
grep -i "error" /var/log/app.log | tail -20
```

## 13. Compression and archives

```bash
tar -cvf archive.tar folder/          # Create tar archive
tar -xvf archive.tar                  # Extract tar archive
tar -czvf archive.tar.gz folder/      # Create gzip-compressed tar
tar -xzvf archive.tar.gz              # Extract gzip-compressed tar
zip -r archive.zip folder/            # Create zip archive
unzip archive.zip                     # Extract zip archive
```

## 14. Environment variables and shell basics

```bash
echo $HOME                       # Show home directory
export MY_VAR="hello"           # Set a variable for this session
env                              # List environment variables
echo $PATH                       # Directories searched for commands
which python3                    # Location of a command
alias ll='ls -la'                # Create an alias for this session
```

To make aliases or variables persistent, add them to your shell's configuration file, such as `~/.bashrc` or `~/.zshrc`.

## 15. Basic shell scripting

```bash
#!/bin/bash

echo "Starting deployment..."
APP_DIR="/home/ec2-user/app"

if [ -d "$APP_DIR" ]; then
  echo "Directory exists, updating..."
  cd "$APP_DIR"
else
  echo "Creating directory..."
  mkdir -p "$APP_DIR"
fi

for file in *.log; do
  echo "Found log: $file"
done

echo "Deployment complete!"
```

Save it as `deploy.sh`, then run:

```bash
chmod +x deploy.sh
./deploy.sh
```

## 16. Redirection and pipes

```bash
command > file.txt                 # Save output, replacing file contents
command >> file.txt                # Append output
command < file.txt                 # Use a file as command input
command1 | command2                # Send output from command1 to command2
```

Example:

```bash
grep "error" app.log | wc -l > error_count.txt
```

## 17. Common cloud and DevOps command combinations

```bash
# Show the ten largest top-level directories
sudo du -sh /* 2>/dev/null | sort -rh | head -10

# Follow a log and show only errors
tail -f /var/log/app.log | grep -i error

# Find a process, then stop it with its PID
ps aux | grep nginx
sudo kill <PID>

# Check open ports
sudo ss -tulpn

# Manage a systemd service
sudo systemctl status nginx
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl enable nginx
```

## Safety reminders

- Inspect a folder with `ls` before using `rm -r`.
- Use `sudo` only when you understand why administrator access is required.
- Use `tail -f` and `journalctl` to investigate application problems before changing settings.
- Never share private SSH key files such as `.pem` files.


