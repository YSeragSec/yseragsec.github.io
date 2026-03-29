---
title: "Linux Basics for Hackers — Week 2 Notes"
date: 2026-03-28 00:00:00 +0200
categories: [CAT Entry Level, Linux]
tags: [linux, bash, kali, networking, permissions, scripting, services, privacy, fundamentals]
description: My Week 2 notes from the CAT Reloaded entry-level program. Covers Linux basics from OccupyTheWeb's book — navigation, text manipulation, networking, permissions, processes, scripting, services, anonymity, wireless, and job scheduling.
last_modified_at: 2026-03-28 00:00:00 +0200
---

<p style="font-size: 1.15em;">These are my Week 2 notes from the <b>CAT Reloaded</b> entry-level program, based on <a href="https://drive.google.com/file/d/1K1HAnhEiAErb43LcwN01vkv6TI2_8HKZ/view?usp=sharing">Linux Basics for Hackers</a> by OccupyTheWeb. Chapters 11, 15, and 17 were skipped per the program.</p>

---

![](/assets/img/posts/CATEntryLevel/Linux/image37.png)

## Chapter 1 — Basic Commands

### 1. Navigation

#### `pwd` — Show current directory
```bash
pwd
```
- Prints full path of current location.
- Use before modifying files.

#### `whoami` — Show current user
```bash
whoami
```
- Confirms if you're `root` or another user.
- Critical for privilege-dependent tools.

#### `cd` — Change directory
```bash
cd /etc
cd ..
cd /
```
- `..` → Up one level
- `/` → Filesystem root

#### `ls` — List contents
```bash
ls
ls -l
ls -la
ls -a
```
- `-l` → Long format (permissions, owner, size, date)
- `-a` → Show hidden files

---

### 2. Help & Documentation

```bash
tool --help
tool -h
man toolname   # q to quit
```

---

### 3. Search & Locate

```bash
locate filename          # Fast, database-based, may miss recent files
whereis toolname         # Binary + man page location
which toolname           # Executable in $PATH only
find / -type f -name filename          # Precise, slow
find /etc -type f -name apache2.*
```

**Wildcards:** `*` = any chars, `?` = one char, `[ab]` = listed chars

---

### 4. Processes & Filtering

```bash
ps aux                   # List all processes
ps aux | grep apache2    # Filter by name
```

---

### 5. File Creation

```bash
cat > file      # Create/overwrite (CTRL+D to save)
cat >> file     # Append
touch file      # Create empty file
```

---

### 6. File & Directory Management

```bash
mkdir dir
cp source destination
mv old new
mv file /path/
rm file
rmdir dir       # Only empty directories
rm -r dir       # Deletes everything inside (dangerous)
```

**Key Distinctions:**
- `locate` = fast, indexed
- `find` = slow, powerful
- `>` overwrites, `>>` appends
- `rm -r` can wipe entire trees

---

## Chapter 2 — Text Manipulation

### Viewing Files

```bash
cat file        # Dump entire file (bad for large files)
head file       # First 10 lines
head -20 file   # First 20 lines
tail file       # Last 10 lines
tail -20 file   # Last 20 lines
nl file         # Number lines
more file       # Page-by-page (ENTER = next, q = quit)
less file       # Scroll up/down + search with /keyword
```

### Filtering Text

```bash
grep keyword file
cat file | grep keyword
ps aux | grep apache2
```

### Context Extraction

```bash
nl file | grep "pattern"
tail -n+507 file | head -n6   # Start from line 507, show 6 lines
```

### Find & Replace with `sed`

```bash
sed 's/mysql/MySQL/g' file > newfile    # Replace all
sed 's/mysql/MySQL/' file > newfile     # Replace first only
sed 's/mysql/MySQL/2' file > newfile    # Replace 2nd match only
```

**Structure:** `s/old/new/g` — `s` = substitute, `g` = global

### Tool Comparison

| Command | Best For |
|---|---|
| `cat` | Quick full output |
| `head` | Beginning of file |
| `tail` | End of file / logs |
| `nl` | Line references |
| `grep` | Filtering lines |
| `sed` | Replace text |
| `more` | Basic paged viewing |
| `less` | Scrolling + search |

> `grep` is your primary filter. `less` is your primary viewer. Use pipes (`|`) to chain commands.
{: .prompt-tip }

---

## Chapter 3 — Analyzing & Managing Networks

### View Network Interfaces

```bash
ifconfig        # Shows IP, MAC, netmask, broadcast
iwconfig        # Wireless details (mode, ESSID, signal)
```

**Key Interfaces:** `eth0` = Wired, `wlan0` = Wireless, `lo` = Loopback (127.0.0.1)

### Change Network Configuration

```bash
ifconfig eth0 192.168.1.10                                      # Assign static IP
ifconfig eth0 192.168.1.10 netmask 255.255.0.0 broadcast 192.168.1.255
```

### Spoof MAC Address

```bash
ifconfig eth0 down
ifconfig eth0 hw ether 00:11:22:33:44:55
ifconfig eth0 up
```

> MAC spoofing requires: interface down → change → up.
{: .prompt-info }

### DHCP Management

```bash
dhclient eth0   # Request new DHCP address
```

Sends DHCPDISCOVER → receives DHCPOFFER → assigns IP. Note: DHCP logs can be used for forensic tracing.

### DNS Reconnaissance

```bash
dig example.com ns    # Find nameservers
dig example.com mx    # Find mail servers
```

### Change DNS Server

```bash
echo "nameserver 8.8.8.8" > /etc/resolv.conf
```

File: `/etc/resolv.conf` — order matters (top = queried first). DHCP may overwrite this.

### Hosts File Manipulation

```bash
leafpad /etc/hosts
# Add: 192.168.1.10 example.com
```

File: `/etc/hosts` — overrides DNS locally. Use for traffic redirection, local testing, DNS spoofing scenarios.

### Tool Summary

| Tool | Purpose |
|---|---|
| `ifconfig` | View/modify IP, MAC, netmask |
| `iwconfig` | Wireless interface info |
| `dhclient` | Request DHCP IP |
| `dig` | DNS intelligence |
| `/etc/resolv.conf` | Set DNS servers |
| `/etc/hosts` | Manual domain-IP mapping |

---

## Chapter 4 — Adding & Removing Software

```bash
apt-cache search <keyword>   # Search packages
apt-get install <package>    # Install
apt-get remove <package>     # Remove (keep config)
apt-get purge <package>      # Remove + config
apt-get update               # Update package list
apt-get upgrade              # Upgrade installed packages
```

**Add repositories:** Edit `/etc/apt/sources.list` → run `apt-get update`

**Install from GitHub:**
```bash
git clone https://github.com/user/project.git
```

Use `apt` first. Git when it's not in repos.

---

## Chapter 5 — Controlling File & Directory Permissions

### Permission Basics

Each file has 3 sets: **owner | group | others** — each set = `r w x`

```bash
ls -l
# Example output: -rw-r--r--
# - = file, rw- = owner, r-- = group, r-- = others
```

### Change Ownership

```bash
chown user filename
chgrp group filename
```

### Numeric Permissions (Fastest)

| rwx | Value |
|---|---|
| `---` | 0 |
| `--x` | 1 |
| `-w-` | 2 |
| `-wx` | 3 |
| `r--` | 4 |
| `r-x` | 5 |
| `rw-` | 6 |
| `rwx` | 7 |

```bash
chmod 774 file    # owner=rwx, group=rwx, others=r--
chmod 755 tool    # owner=rwx, group=r-x, others=r-x
```

### Symbolic Permissions (UGO)

```bash
chmod u-w file           # Remove write from owner
chmod u+x,o+x file       # Add execute to owner and others
chmod +x tool            # Make executable
```

### Default Permissions — umask

Base: Files=666, Directories=777. System subtracts umask.

```bash
umask        # Check current (typically 022 on Kali)
umask 007    # Set temporarily
```

Kali default result: Files → 644, Directories → 755

### Special Permissions

**SUID (4000)** — Runs with owner's privileges:
```bash
chmod 4644 file    # Shows as -rwsr-xr-x
find / -user root -perm -4000    # Find SUID files (privesc hunting)
```

**SGID (2000)** — Runs with group privileges:
```bash
chmod 2644 file
```

**Sticky Bit (1000)** — Used on `/tmp` — prevents users from deleting others' files.

> Bad permissions = easy privilege escalation. Loose SUID/SGID = attack surface.
{: .prompt-warning }

---

## Chapter 6 — Process Management

```bash
ps            # Basic view
ps aux        # Full system view (PID, %CPU, %MEM, COMMAND)
ps aux | grep msfconsole    # Filter for specific process
top           # Live view, sorted by CPU (q=quit, k=kill, r=renice)
```

### Priority (nice / renice)

Range: **-20 (highest)** to **+19 (lowest)**. Default = 0. Only root can go negative.

```bash
nice -n -10 command    # Start with priority
renice 19 PID          # Change running process
```

### Kill a Process

```bash
kill PID              # Clean stop
kill -9 PID           # Force kill
killall -9 processname
```

### Background & Scheduling

```bash
command &     # Run in background
fg            # Bring back to foreground
at 1:00am     # Schedule one-time (then type command, CTRL+D)
# Formats: "at now + 20 minutes", "at 7:30pm", "at tomorrow"
```

---

## Chapter 7 — Environment Variables

```bash
env                     # Show environment variables
set | more              # Show everything
set | grep HISTSIZE     # Filter for one
HISTSIZE=0              # Change session only
export HISTSIZE         # Make persistent
unset MYVAR             # Delete variable
```

### PATH (Critical)

```bash
echo $PATH
PATH=$PATH:/root/newtool    # Correct — always append
# PATH=/root/newtool        # Wrong — breaks system commands
```

### Change Shell Prompt (PS1)

```bash
PS1="Hacker:# "
export PS1='C:\w> '     # Windows-style
# Placeholders: \u=user, \h=hostname, \W=current dir
```

---

## Chapter 8 — Bash Scripting

```bash
#!/bin/bash
echo "Hello, World!"
```

```bash
chmod 755 script.sh
./script.sh
```

### Variables + User Input

```bash
#!/bin/bash
echo "What is your name?"
read name
echo "Welcome $name!"
```

### Port Scanner Example

```bash
#!/bin/bash
# Basic MySQL scanner
nmap -sT 192.168.1.0/24 -p 3306 > /dev/null -oG scan
cat scan | grep open > results
cat results
```

**Advanced (user-driven):**
```bash
#!/bin/bash
echo "Start IP:"; read FirstIP
echo "End IP:"; read LastIP
echo "Port:"; read port
nmap -sT $FirstIP-$LastIP -p $port > /dev/null -oG scan
cat scan | grep open > results
cat results
```

### Built-ins

| Command | Purpose |
|---|---|
| `echo` | Print |
| `read` | Input |
| `export` | Persist variable |
| `unset` | Remove variable |
| `test / [ ]` | Condition checks |
| `jobs` | List background tasks |
| `bg / fg` | Background / foreground |

---

## Chapter 9 — Compressing & Archiving

```bash
tar -cvf L4H.tar file1 file2    # Archive (c=create, v=verbose, f=filename)
tar -tvf L4H.tar                # List contents
tar -xvf L4H.tar                # Extract

gzip L4H.tar      → L4H.tar.gz    # Compress (fast, common)
gunzip L4H.tar.gz                  # Decompress

bzip2 L4H.tar     → L4H.tar.bz2   # Best compression, slower
bunzip2 L4H.tar.bz2
```

**One-step (real-world way):**
```bash
tar -czvf L4H.tar.gz Linux4Hackers*    # gzip
tar -cjvf L4H.tar.bz2 Linux4Hackers*  # bzip2
```

### Forensic Copy — `dd`

```bash
dd if=/dev/sdb of=/root/flashcopy bs=4096 conv=noerror
```

Bit-for-bit copy including deleted data. Very slow. Use only for full physical clones.

---

## Chapter 10 — Filesystem & Storage Device Management

### Drive Naming

- `sda` → first drive, `sdb` → second, `sda1` → first partition on first drive

```bash
lsblk       # List drives (device, size, type, mount)
fdisk -l    # Detailed partition view (root required)
```

### Mount / Unmount

```bash
mount /dev/sdb1 /mnt       # Mount
umount /dev/sdb1            # Unmount before removal (no 'n'!)
df -h                       # Check disk space
```

### Check Filesystem

```bash
umount /dev/sdb1
fsck -p /dev/sdb1    # Auto-fix. Never run on mounted drives.
```

| Task | Command |
|---|---|
| List drives | `lsblk` |
| See partitions | `fdisk -l` |
| Check space | `df -h` |
| Mount | `mount` |
| Unmount | `umount` |
| Check errors | `fsck` |
| Clone drive | `dd` |

---

## Chapter 12 — Using & Abusing Services

```bash
service <name> start/stop/restart
systemctl start/stop/restart/status <name>
```

### Apache HTTP Server

```bash
apt install apache2
service apache2 start
# Test: http://localhost
# Edit: /var/www/html/index.html
```

### OpenSSH

```bash
service ssh start
ssh user@IP
ssh pi@192.168.1.101
```

Used for: remote control, secure admin access, pivoting after compromise.

### MySQL

```bash
service mysql start
mysql -u root -p

show databases;
use mysql;
show tables;
SELECT * FROM table_name;
ALTER USER 'root'@'localhost' IDENTIFIED BY 'newpassword';
```

### PostgreSQL + Metasploit

```bash
service postgresql start
msfconsole
msfdb init
db_status
```

> Without PostgreSQL → Metasploit has limited functionality.
{: .prompt-info }

**What each service does:**
- Apache = serve web content
- SSH = remote control
- MySQL = web app data
- PostgreSQL = Metasploit backend

---

## Chapter 13 — Becoming Secure & Anonymous

### How You Get Tracked
- IP address tags every packet
- ISPs log everything
- Sites fingerprint your browser

```bash
traceroute google.com    # See every hop your traffic takes
```

### Tor Browser

<p style="font-size: 1.15em;">Routes traffic through volunteer relays with multi-layer encryption. Hides origin IP from destination.</p>

**Reality:** Slower. Exit nodes can see unencrypted traffic. Nation-state agencies target it. Good for basic tracking avoidance — not bulletproof.

### Proxy Chains

```bash
proxychains firefox example.com
# Config: /etc/proxychains.conf
# Modes: dynamic_chain, strict_chain, random_chain
```

> Free proxies log you. Many sell data. If it's free, you're the product.
{: .prompt-warning }

### VPNs

Encrypts traffic and replaces your IP. VPN provider still sees your real IP — logs = exposure. Good for ISP privacy and public Wi-Fi, not true anonymity.

### What Actually Improves Privacy

- VPN + Tor combined
- Encrypted email (ProtonMail)
- Hardened browser
- Separate identities per activity
- Never mix real-world info with anonymous ops

---

## Chapter 14 — Understanding & Inspecting Wireless Networks

### Wi-Fi Basics

- **SSID** — network name
- **BSSID** — AP MAC address
- **Channel** — frequency slot (1–11 US)
- **Security** — WEP (broken), WPA, WPA2-PSK
- **Modes** — managed (normal), master (AP), monitor (sniffing)
- **Frequency** — 2.4GHz / 5GHz

### Core Commands

```bash
ifconfig / iwconfig            # Check interfaces
iwlist wlan0 scan              # Scan nearby APs (raw)
nmcli dev wifi                 # Scan (cleaner output)
nmcli dev wifi connect SSID password PASSWORD
```

### Wi-Fi Recon with aircrack-ng

```bash
airmon-ng start wlan0          # Enable monitor mode → wlan0mon
airodump-ng wlan0mon           # Capture traffic (BSSID, channel, encryption, clients)
```

Monitor mode = see all traffic, not just yours.

### Bluetooth Recon

```bash
hciconfig                      # Check adapter
hciconfig hci0 up
hcitool scan                   # Scan discoverable devices
hcitool inq
sdptool browse MAC             # Get service details
l2ping MAC                     # Test reachability
```

> Enumeration comes first. Exploitation comes later.
{: .prompt-tip }

---

## Chapter 16 — Automating Tasks with Job Scheduling

### cron — Time-Based Execution

```bash
crontab -e    # Edit user cron
# Config: /etc/crontab
```

**Format:** `M H DOM MON DOW USER COMMAND`

```
Minute    0–59
Hour      0–23
Day/Month 1–31
Month     1–12
Day/Week  0–7 (0 & 7 = Sunday)
```

**Examples:**
```bash
30 2 * * 1-5 root /root/myscript.sh      # 2:30 AM Mon–Fri
00 15 * * 3 user /usr/share/script.sh    # Wednesday 3 PM
00 0 10 4,6,8 * user /usr/share/script.sh # 10th day April/June/Aug
```

**Shortcuts:**
```
@daily    @weekly    @monthly    @yearly    @reboot
```

### Services at Boot (systemd)

```bash
systemctl enable postgresql     # Start at boot
systemctl disable postgresql    # Disable
systemctl status postgresql     # Check
update-rc.d postgresql defaults # Old method
```

> Always use absolute paths in cron scripts. Test manually before scheduling.
{: .prompt-warning }

---

***If you have any questions, feel free to reach out on [LinkedIn](https://www.linkedin.com/in/yousefserag/) or [Discord](https://discord.com/users/751180791492378774)***
