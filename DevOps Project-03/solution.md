# Linux User Management & File System Administration on AWS EC2

## Project Overview

This project demonstrates practical Linux system administration tasks on an AWS EC2 instance.
The tasks cover:

* Linux user and group management
* File and directory permissions
* Ownership and access control
* File operations and navigation
* Text processing with Linux commands
* Vi editor operations
* EBS volume creation and mounting
* File system management
* Cleanup and resource termination in AWS

This assignment simulates real-world responsibilities of a DevOps Engineer working with Linux servers in cloud environments.

---

# Architecture

## AWS Resources Used

* AWS EC2 Instance (Linux)
* AWS EBS Volume (5GB)

---

# Technologies & Tools

* AWS EC2
* AWS EBS
* Linux (Ubuntu/Amazon Linux/CentOS)
* Bash Commands
* Vi Editor
* File System Utilities

---

# Prerequisites

Before starting:

1. AWS Account
2. EC2 Key Pair
3. Linux-based EC2 instance running
4. SSH access to the instance
5. Basic understanding of Linux commands

---

# Connect to EC2 Instance

```bash
ssh -i your-key.pem ec2-user@<PUBLIC_IP>
```

For Ubuntu:

```bash
ssh -i your-key.pem ubuntu@<PUBLIC_IP>
```

Switch to root user:

```bash
sudo su -
```

---

# TASK 1 — Root User Operations

---

## 1. Create Users

```bash
useradd user1
passwd user1

useradd user2
passwd user2

useradd user3
passwd user3
```

---

## 2. Create Groups

```bash
groupadd devops
groupadd aws
```

---

## 3. Change Primary Group of user2 and user3

```bash
usermod -g devops user2
usermod -g devops user3
```

---

## 4. Add Secondary Group to user1

```bash
usermod -aG aws user1
```

---

## 5. Create Directory Structure

Example structure:

```bash
mkdir /dir1
mkdir -p /dir2/dir1/dir2/dir10
mkdir /dir3
mkdir /dir4
mkdir /dir5
mkdir -p /dir6/dir4
mkdir -p /dir7/dir10
mkdir /dir8
mkdir -p /opt/dir14/dir10
```

Create files:

```bash
touch /dir1/f1
touch /f1
touch /f2
touch /f3
```

---

## 6. Change Group Ownership

```bash
chgrp devops /dir1
chgrp devops /dir7/dir10
chgrp devops /f2
```

---

## 7. Change User Ownership

```bash
chown user1 /dir1
chown user1 /dir7/dir10
chown user1 /f2
```

---

# TASK 2 — Operations as user1

Switch user:

```bash
su - user1
```

---

## 1. Create Users

```bash
sudo useradd user4
sudo passwd user4

sudo useradd user5
sudo passwd user5
```

> NOTE: user1 must have sudo privileges for this operation.

Grant sudo access from root:

```bash
usermod -aG wheel user1
```

Ubuntu:

```bash
usermod -aG sudo user1
```

---

## 2. Create Groups

```bash
sudo groupadd app
sudo groupadd database
```

---

# TASK 3 — Operations as user4

Switch user:

```bash
su - user4
```

---

## 1. Create Directory

```bash
mkdir -p /dir6/dir4
```

---

## 2. Create File

```bash
touch /f3
```

---

## 3. Move File

```bash
mv /dir1/f1 /dir2/dir1/dir2
```

---

## 4. Rename File

```bash
mv /f2 /f4
```

---

# TASK 4 — Operations as user1

Switch user:

```bash
su - user1
```

---

## 1. Create Directory

```bash
mkdir /home/user2/dir1
```

---

## 2. Create File Using Relative Path

Navigate:

```bash
cd /dir2/dir1/dir2/dir10
```

Create file:

```bash
touch ../../../../../opt/dir14/dir10/f1
```

---

## 3. Move File to Home Directory

```bash
mv /opt/dir14/dir10/f1 ~
```

---

## 4. Delete /dir4 Recursively

```bash
rm -rf /dir4
```

---

## 5. Delete Child Files and Directories Under /opt/dir14

```bash
rm -rf /opt/dir14/*
```

---

## 6. Write Text to /f3

```bash
echo "Linux assessment for an DevOps Engineer!! Learn with Fun!!" > /f3
```

---

# TASK 5 — Operations as user2

Switch user:

```bash
su - user2
```

---

## 1. Create File

```bash
touch /dir1/f2
```

---

## 2. Delete /dir6

```bash
rm -rf /dir6
```

---

## 3. Delete /dir8

```bash
rm -rf /dir8
```

---

## 4. Replace “DevOps” with “devops”

```bash
sed -i 's/DevOps/devops/g' /f3
```

---

## 5. Copy Line in Vi Editor

Open file:

```bash
vi /f3
```

Inside vi editor:

```text
yy
10p
```

Explanation:

* `yy` → copy current line
* `10p` → paste 10 times

---

## 6. Replace “Engineer” with “engineer”

```bash
sed -i 's/Engineer/engineer/g' /f3
```

---

## 7. Delete /f3

```bash
rm -f /f3
```

---

# TASK 6 — Operations as root

Switch:

```bash
sudo su -
```

---

## 1. Search for File f3

```bash
find / -name f3 2>/dev/null
```

---

## 2. Count Number of Files in /

```bash
find / -type f | wc -l
```

---

## 3. Print Last Line of /etc/passwd

```bash
tail -n 1 /etc/passwd
```

---

# TASK 7 — Create and Attach EBS Volume

---

## AWS Steps

1. Open AWS Console
2. Navigate to EC2
3. Click Volumes
4. Create Volume

   * Size: 5GB
   * Same Availability Zone as EC2
5. Attach Volume to Instance

---

# TASK 8 — File System Operations

---

## 1. Check Attached Disk

```bash
lsblk
```

Example new disk:

```text
/dev/xvdf
```

---

## 2. Create File System

```bash
mkfs -t ext4 /dev/xvdf
```

---

## 3. Create Mount Directory

```bash
mkdir /data
```

---

## 4. Mount File System

```bash
mount /dev/xvdf /data
```

---

## 5. Verify Mount

```bash
df -h
```

Expected output should contain:

```text
/data
```

---

## 6. Create File

```bash
touch /data/f1
```

---

# TASK 9 — Operations as user5

Switch user:

```bash
su - user5
```

---

## Delete Directories and Files

```bash
rm -rf /dir1
rm -rf /dir2
rm -rf /dir3
rm -rf /dir5
rm -rf /dir7

rm -f /f1
rm -f /f4

rm -rf /opt/dir14
```

---

# TASK 10 — Cleanup as root

Switch:

```bash
sudo su -
```

---

## 1. Delete Users

```bash
userdel -r user1
userdel -r user2
userdel -r user3
userdel -r user4
userdel -r user5
```

---

## 2. Delete Groups

```bash
groupdel app
groupdel aws
groupdel database
groupdel devops
```

---

## 3. Delete Home Directories

```bash
rm -rf /home/user1
rm -rf /home/user2
rm -rf /home/user3
rm -rf /home/user4
rm -rf /home/user5
```

---

## 4. Unmount File System

```bash
umount /data
```

---

## 5. Delete /data Directory

```bash
rm -rf /data
```

---

# TASK 11 — AWS Resource Cleanup

---

## Detach and Delete EBS Volume

1. Go to EC2 Console
2. Select Volumes
3. Detach Volume
4. Delete Volume

---

## Terminate EC2 Instance

1. Select EC2 Instance
2. Instance State
3. Terminate Instance

---

# Verification Commands

## Check Users

```bash
cat /etc/passwd
```

---

## Check Groups

```bash
cat /etc/group
```

---

## Check Ownership

```bash
ls -l /
```

---

## Check Mounted File Systems

```bash
df -h
```

---

# Key Linux Concepts Covered

* User Management
* Group Management
* File Permissions
* Ownership
* Relative vs Absolute Paths
* File Manipulation
* Linux Text Processing
* Vi Editor Usage
* File System Creation
* EBS Mounting
* Linux Administration
* AWS Infrastructure Management

---

# Common Commands Used

| Command  | Purpose                  |
| -------- | ------------------------ |
| useradd  | Create users             |
| passwd   | Set password             |
| groupadd | Create groups            |
| usermod  | Modify users/groups      |
| mkdir    | Create directories       |
| touch    | Create files             |
| mv       | Move/Rename files        |
| rm       | Delete files/directories |
| chown    | Change ownership         |
| chgrp    | Change group             |
| sed      | Replace text             |
| find     | Search files             |
| tail     | Print last lines         |
| mkfs     | Create filesystem        |
| mount    | Mount filesystem         |
| df -h    | Disk usage               |

---

# Learning Outcomes

After completing this project, you should understand:

* Linux system administration basics
* User and permission management
* Linux file systems
* EBS storage handling in AWS
* Shell command usage
* Linux file operations
* DevOps operational practices

---

# Author

John Bankole
DevOps Engineer
