# Linux Commands I Learned Today

## 1. Moving Around

### `pwd`
Shows the current folder location.

```bash
pwd
```

### `ls`
Lists files and folders.

```bash
ls
ls -la
```

- `-l` shows details such as permissions, owner, and size.
- `-a` includes hidden files.

### `cd`
Moves between folders.

```bash
cd folder-name      # Enter a folder
cd ..               # Move up one folder
cd ~                # Go to home folder
cd /var/log         # Go to an exact location
```

## 2. Creating Files and Folders

### `mkdir`
Creates a folder.

```bash
mkdir project
mkdir -p project/logs/archive
```

`-p` creates parent folders when needed.

### `touch`
Creates an empty file.

```bash
touch app.log
touch notes.txt
```

## 3. Copying, Moving, and Deleting

### `cp`
Copies files.

```bash
cp source.txt backup.txt
```

### `cp -r`
Copies a folder and everything inside it.

```bash
cp -r project project-backup
```

### `mv`
Renames or moves files.

```bash
mv old.txt new.txt
mv app.log logs/
```

### `rm`
Deletes a file permanently.

```bash
rm unwanted.txt
rm -i unwanted.txt
```

`-i` asks for confirmation before deletion.

### `rm -r`
Deletes a folder and everything inside it.

```bash
rm -r old-folder
rm -ri old-folder
```

Be careful: terminal deletions usually do not go to a recycle bin.

## 4. Reading Files and Logs

### `cat`
Prints the whole file. Best for short files.

```bash
cat file.txt
```

### `less`
Opens a file one page at a time.

```bash
less large-file.log
```

Press `q` to exit.

### `head`
Shows the first lines of a file.

```bash
head -n 10 file.txt
```

### `tail`
Shows the last lines of a file.

```bash
tail -n 20 app.log
```

### `tail -f`
Watches a log file as new lines are added.

```bash
tail -f app.log
```

Press `Control + C` to stop watching.

## 5. Cloud Server Basics

### Server information

```bash
whoami        # Current user
hostname      # Server name
uptime        # Server running time and load
```

### Disk and memory

```bash
df -h         # Disk space
free -h       # Memory use
top           # Live CPU and memory usage
```

Press `q` to exit `top`.

### Running processes

```bash
ps aux
ps aux | grep nginx
```

### Networking

```bash
ip addr
ping google.com
curl https://example.com
curl -I https://example.com
ss -tulpn
```

Press `Control + C` to stop `ping`.

## 6. Connecting to a Cloud Server

```bash
ssh username@server-ip-address
```

Example:

```bash
ssh ubuntu@203.0.113.10
```

Using an SSH key:

```bash
ssh -i my-key.pem ubuntu@203.0.113.10
```

Copy a file to a server:

```bash
scp -i my-key.pem app.zip ubuntu@203.0.113.10:/home/ubuntu/
```

## 7. Installing Software

For Ubuntu and Debian:

```bash
sudo apt update
sudo apt upgrade
sudo apt install nginx
```

For Amazon Linux, RHEL, or CentOS:

```bash
sudo dnf install nginx
```

## 8. Managing Services

```bash
sudo systemctl status nginx
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl enable nginx
```

## 9. Permissions

```bash
ls -l
chmod +x script.sh
chmod 600 my-key.pem
sudo chown ubuntu:ubuntu app.log
```

## 10. Troubleshooting a Cloud App

```bash
sudo systemctl status nginx
sudo journalctl -u nginx -n 50
sudo journalctl -u nginx -f
ss -tulpn
df -h
free -h
```

Always be careful with `sudo`, `rm`, and changes to system folders.
