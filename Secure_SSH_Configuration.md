# Secure SSH Configuration

SSH (Secure Shell) is a widely used protocol for securely accessing remote systems. Misconfigurations in SSH can lead to unauthorized access and compromise system security. This guide outlines the best practices for configuring SSH securely.

---

## **Key Points for Secure SSH Configuration**

### 1. Disable Root Login
Allowing root login over SSH increases the risk of brute-force attacks. It is recommended to disable direct root login and use a non-root user with `sudo` privileges instead.

#### Steps to Disable Root Login:
1. Open the SSH configuration file:
   ```bash
   sudo nano /etc/ssh/sshd_config
2. Find the line:
   PermitRootLogin yes
   
3. Change it to:
    ```bash
   PermitRootLogin no
   
5. Save the file and restart the SSH service:
   ```bash
   sudo systemctl restart sshd

### 2. **Enable Key-Based Authentication**
Using key-based authentication is more secure than password-based authentication. It requires a private-public key pair to establish a connection.
#### Steps to Enable Key-Based Authentication:
1.Generate an SSH key pair on your client machine:
```bash
  ssh-keygen -t rsa -b 4096

2. Copy the public key to the server:
```bash
    ssh-copy-id username@server_ip

3. Open the SSH configuration file:
```bash
    sudo nano /etc/ssh/sshd_config

4. Ensure the following lines are set:
```bash
    PubkeyAuthentication yes
    PasswordAuthentication no

5. Restart the SSH service:
```bash
    sudo systemctl restart sshd

### 3. **Restrict SSH Access by IP Address**
Restricting SSH access to specific IP addresses limits the attack surface.
#### Steps to Restrict Access:
1. Open the SSH configuration file:
```bash
    sudo nano /etc/ssh/sshd_config

2. Add the following lines, replacing 0.0.0.0/24 with your trusted network:
```bash
    AllowUsers username@0.0.0.0/24

3. Restart the SSH service:
```bash
    sudo systemctl restart sshd


### **4. Use Strong SSH Keys**
Weak SSH keys can be brute-forced. Ensure your keys meet the following criteria:

Minimum length: 2048 bits (4096 bits recommended for RSA).
Use newer algorithms like ed25519 for better security and performance.
#### Steps to Generate a Strong Key:
1. Use the following command to generate an ed25519 key:
```bash
    ssh-keygen -t ed25519 -a 100
2. Follow the on-screen instructions to complete key generation.

3. Change the default SSH port to reduce scanning attempts.
```bash
     sudo nano /etc/ssh/sshd_config
     # Change the line:
        Port 22
     # To something like:
        Port 2222

