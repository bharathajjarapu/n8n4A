# n8n4A
n8n on Android

### Contents
1. [Termux on Android](https://github.com/bharathajjarapu/n8n4A/edit/main/README.md#first-install-termux-on-android)
2. 


### First Install Termux on Android

### Setup SSH on Termux

#### Firstly Install and Start OpenSSH on Termux

- Update Termux packages and install OpenSSH:
  ```bash
  pkg update && pkg install openssh
  ```

- Start the SSH server:
  ```bash
  sshd
  ```

#### Then Set a Password for Termux

Set a password for SSH authentication:
```bash
passwd
```

#### Find the IP Address and Username

Retrieve your Android device's IP address:
```bash
ifconfig
```
Look under the `wlan0` interface for the `inet` address (e.g., `192.168.1.5`).  
Find your Termux username using:
```bash
whoami
```

#### Connect to Termux from Laptop/PC

Use the SSH command from your Phone to Directly get the Connection Command:
```bash
cat > ~/sshme << 'EOF'
#!/bin/bash

USER=$(whoami)

IP=$(ip addr 2>/dev/null | grep -oP '(?<=inet\s)\d+\.\d+\.\d+\.\d+' \
     | grep -v '^127' | head -1)

echo
echo "Your IP Address is $IP"
echo
echo "Your SSH command:"
echo "ssh $USER@$IP"
echo
EOF

chmod +x ~/sshme
```
Example:  
```bash
sshme
```
You will get something like this `ssh -p 8022 u0_a123@192.168.1.5`
Then Run it
```bash
ssh -p 8022 u0_a123@192.168.1.5
```

### Lets start Building Termux

#### Firstly Update Termux packages and add root repository
```
pkg install root-repo
pkg update -y
pkg upgrade -y
```

#### Install Python 3.10
```
pkg install python310
```

#### Install NodeJS LTS (16.x)
```
pkg install nodejs-lts
```

#### Install dependencies for compiling SQLite from source
```
pkg install clang libsqlite pkg-config python make binutils
export GYP_DEFINES="android_ndk_path=''"
pip install setuptools
```

#### Install SQLite3 from source
```
npm install sqlite3 --build-from-source --sqlite=/data/data/com.termux/files/usr/bin/sqlite3 -g
```

#### Install n8n globally
```
npm install n8n -g --sqlite=/data/data/com.termux/files/usr/bin/sqlite3
```

#### Install PM2 to manage n8n as a background process
```
npm install pm2 -g
```

---

### Running n8n Directly

#### Start n8n without Tunnel (webhooks on local network only)
```
pm2 start n8n
```

#### Start n8n with Tunnel (for webhooks, not recommended for production)
```
pm2 start n8n --tunnel
```

#### Save PM2 process list for persistence
```
pm2 save
```

#### Check the status of PM2 processes
```
pm2 status
```

### Network Setup

#### Install net-tools to check Termux IP
```
pkg install net-tools
```

#### Display network interfaces and IP address
```
ifconfig
```

#### Access n8n in your browser
```
http://IP:5678
```

Replace `$IP` with the IP address from `ifconfig`.

### 
