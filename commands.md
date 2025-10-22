# commands-linux  

Welcome to the **commands-linux** repo, your collection of **super useful CLI commands and bash aliases** for everyday Linux tasks, audio transcript cleanup, network troubleshooting, blue team security, and file encryption—all explained simply! 🚀🐧

***

## 🎧 Bash Aliases for Transcript Cleanup

Add these aliases to your `~/.bashrc` or `~/.bash_profile` for quick transcript editing:

```bash
# Remove timestamps from transcript files (e.g., 00:MM:SS or 00:MM:SS,mmm)
alias notime='sed "/^00:[0-5][0-9]:[0-5][0-9]/d; /^-->/d; /^[0-9]*$/d"'
# Usage: notime file.srt > output.txt

# Delete blank lines from transcript files
alias noblank='sed "/^$/d"'
# Usage: noblank file.srt > output.txt

# Remove both timestamps and blank lines
alias tclean='sed "/^00:[0-5][0-9]:[0-5][0-9]/d; /^-->/d; /^[0-9]*$/d; /^$/d"'
# Usage: tclean file.srt > output.txt
```

> **How to add:**  
> 1. Run `nano ~/.bashrc`  
> 2. Paste the aliases at the end  
> 3. Save with Ctrl+O, Enter, exit with Ctrl+X  
> 4. Reload with `source ~/.bashrc`  

***

## 🌐 Quick Network & Docker One-Liners  

### Get your local IP address instantly:  
```bash
ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}'
```

### Automatically print your Pi-hole dashboard URL:  
```bash
ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}' | xargs -I {} echo "Pi-hole URL: http://{}:80/admin"
```

### Show local IP and all listening ports:  
```bash
ip -4 addr show eth0 | grep -oP '(?<=inet\s)\d+(\.\d+){3}' | xargs -I {} sh -c 'echo "IP: {}"; sudo netstat -tuln | grep LISTEN'
```

### List running Docker containers with their IPs:  
```bash
docker ps -q | xargs -I {} docker inspect --format '{{.Name}} - {{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' {}
```

### Test DNS resolution quickly:  
```bash
dig google.com @1.1.1.1 +short
```

***

## 📁 Disk Usage & File Management Commands  

- Show top 10 largest folders in current directory:  
  `du -h --max-depth=1 | sort -hr | head -n 10`

- Zip all files modified in the last 24 hours:  
  `find . -type f -mtime -1 | zip today_files -@`

- Find large files over 100MB anywhere:  
  `find / -type f -size +100M -exec ls -lh {} \;`

- Set all files in current directory to permission 644 (readable by you and others):  
  `find . -type f -exec chmod 644 {} \;`

***

## 🛡️ Blue Team Security Commands  

- Show top 10 CPU-heavy processes not running as root (spot odd activity)  
```bash
ps aux --sort=-%cpu | grep -v "root" | head -n 10
```

- Find potentially sensitive `.pem` and `.key` files (certificates and keys):  
```bash
find / -type f \( -name "*.pem" -o -name "*.key" \) 2>/dev/null
```

- Audit file permissions in sensitive dirs for restrictive setups:  
```bash
find /etc /var /root -type f -exec ls -l {} \; | grep "^..-..-..--"
```

***

## 🔐 Easy File Encryption Commands Explained (with emojis!)  

### 1. Pack, zip, and lock your folder tight 🔐📦  
```bash
tar czf - foldername | gpg -c --cipher-algo AES256 -o foldername.tar.gz.gpg
```
- Packs and compresses your folder  
- Pipes it to GPG for strong password encryption  
- Saves encrypted package  
- **Why?** Keep your data safe! Without the password, it’s just 💩 gibberish for everyone else.

***

### 2. Encrypt a file and save it, but keep options open 🗝️💾  
```bash
gpg -c --cipher-algo AES256 filename | tee filename.gpg
```
- Encrypts your file with password prompt  
- Saves encrypted file AND lets you send it somewhere else if needed  
- **Why?** Save & share safely without extra steps!

***

### 3. Encrypt a big file AND squash it smaller 📦⚡  
```bash
openssl enc -aes-256-cbc -salt -pbkdf2 -in bigfile -out >(gzip > bigfile.enc.gz)
```
- Encrypts using very strong AES-256 encryption  
- Sends output through gzip compressor to shrink size  
- Saves as `bigfile.enc.gz`  
- **Why?** Save space AND keep it locked tight!

***

### 4. Open your locked and squished folder 🔓📂  
```bash
gpg -d foldername.tar.gz.gpg | tar xzvf -
```
- Unlocks your encrypted file  
- Immediately extracts folder contents without messy temp files  
- **Why?** Fast and clean — just how we like it!

***

### What are pipes (`|`) and process substitution (`>(...)`)? 🔄🧙‍♂️

- Pipe `|`: Passes output from one command straight into another without saving it on disk — like a magic tube!  
- Process substitution `>(...)`: Lets output go straight into another command’s input, good for real-time compression or encryption.

***

Feel free to fork, use, and tweak! For help with bash scripting, git management, or more Linux tips, just open an Issue.  
