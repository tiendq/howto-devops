# Setup Ubuntu Guidelines

Setup a fresh and secured Ubuntu instance, additional installation/configuration steps are required to fulfill other deployment requirements.

Create an Ubuntu 22.04 LTS instance, set `root` password, add SSH key for `root` user. All things can be done in UI from any hosting provider e.g. Linode.

Then it's up and ready to SSH and do all necessary configurations.

## Basic Configuration

```bash
ssh root@172.104.51.223

apt update && apt upgrade
timedatectl set-timezone Asia/Ho_Chi_Minh
date
hostnamectl set-hostname NEW_HOST_NAME

# Add below entries into hosts file
# 172.104.51.223 NEW_HOST_NAME
# 2400:8901::f03c:93ff:fee7:375c NEW_HOST_NAME
nano /etc/hosts

adduser NEW_ADMIN_USER
adduser NEW_ADMIN_USER sudo
exit

# SSH again with new admin user, do not use root user from now on
ssh NEW_ADMIN_USER@172.104.51.223
mkdir -p ~/.ssh && sudo chmod -R 700 ~/.ssh/
exit

# Use your public key for SSH in the future
scp ~/.ssh/id_ed25519.pub NEW_ADMIN_USER@172.104.51.223:~/.ssh/authorized_keys

ssh NEW_ADMIN_USER@172.104.51.223
sudo chmod -R 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys

# Change and uncomment these values as below to secure SSH sessions
# AddressFamily inet
# PermitRootLogin no
# PasswordAuthentication no
sudo nano /etc/ssh/sshd_config

sudo systemctl restart sshd
```

## Configure a Firewall

```bash
sudo apt-get install ufw

# Deny all incoming requests by default
sudo ufw default allow outgoing
sudo ufw default deny incoming

# Open only necessary ports, and when you really need them!!!
sudo ufw allow ssh

sudo ufw enable
sudo ufw status
```

## References

[Setting Up and Securing a Compute Instance](https://www.linode.com/docs/guides/set-up-and-secure)

[How to Configure a Firewall with UFW](https://www.linode.com/docs/guides/configure-firewall-with-ufw/)
