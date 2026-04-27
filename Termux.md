# myTC4A
my Termux Configurations for Android Server

## Setup SSH on Termux

### Firstly Install and Start OpenSSH on Termux

- Update Termux packages and install OpenSSH:
  ```bash
  pkg update && pkg install openssh
  ```

- Start the SSH server:
  ```bash
  sshd
  ```

### Then Set a Password for Termux

Set a password for SSH authentication:
```bash
passwd
```

### Find the IP Address and Username

Retrieve your Android device's IP address:
```bash
ifconfig
```
Look under the `wlan0` interface for the `inet` address (e.g., `192.168.1.5`).

Find your Termux username using:
```bash
whoami
```

### Connect to Termux from Laptop/PC

If already have it but messed it up then
```
rm -f ~/.local/bin/sshme
```

Use the SSH command from your Phone to Directly get the Connection Command:
```bash
cat << 'EOF' > ~/.local/bin/sshme
#!/bin/bash

USER=$(whoami)

IP=$(ifconfig 2>/dev/null \
    | awk '/inet / {print $2}' \
    | grep -v '^127' \
    | head -n 1)

echo
if [ -n "$IP" ]; then
    echo "Your IP Address is $IP"
    echo
    echo "Your SSH command:"
    echo "ssh $USER@$IP"
else
    echo "IP address not found"
fi
echo
EOF

chmod +x ~/.local/bin/sshme
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

## 
