# VPSGuard

VPSGuard is a one-click Ubuntu LTS VPS initialization and SSH security hardening tool.

It is designed for a new VPS after the first root login.

## What VPSGuard Does

- Check Ubuntu LTS system
- Update system packages
- Install basic tools
- Create a new sudo user: `vpsguard`
- Copy root SSH public keys to the new user
- Test sudo permission
- Install and configure UFW firewall
- Allow only the current SSH port by default
- Install and configure fail2ban
- Disable root SSH login
- Disable SSH password login
- Keep SSH key login enabled
- Print final configuration with colored output

## Supported System

Ubuntu LTS only.

Recommended:

- Ubuntu 22.04 LTS
- Ubuntu 24.04 LTS

## Important Before Running

Before running VPSGuard, make sure `/root/.ssh/authorized_keys` exists and contains your SSH public key.

Check:

```bash
ls -la /root/.ssh
cat /root/.ssh/authorized_keys
```

If `authorized_keys` is empty, add your SSH public key first.

## Quick Start

Run as root:

```bash
apt update && apt install -y git curl wget
cd /root
git clone https://github.com/hexa46656-creator/vpsguard.git
cd vpsguard
chmod +x install.sh status.sh uninstall.sh
bash install.sh
```

## Default User

The default new user is:

```bash
vpsguard
```

After installation, test login from your local computer:

```bash
ssh vpsguard@YOUR_SERVER_IP -p 22
```

Then test sudo:

```bash
sudo whoami
```

Expected output:

```bash
root
```

## Default Open Ports

VPSGuard only allows the current SSH port by default.

Usually this means:

```bash
22/tcp
```

It does not open `80`, `443`, `8443`, or other service ports automatically.

If you deploy a website later:

```bash
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
```

If you deploy a custom service later:

```bash
sudo ufw allow YOUR_PORT/tcp
```

## Custom User

If you want to use another username:

```bash
NEW_USER=alex bash install.sh
```

## Custom SSH Port

VPSGuard automatically detects the current SSH port.

You can also specify it manually:

```bash
SSH_PORT=22 bash install.sh
```

## Check Status

```bash
bash status.sh
```

## Uninstall

```bash
bash uninstall.sh
```

## Important Warning

Do not close your current root SSH session immediately after running VPSGuard.

Open a new terminal window and test:

```bash
ssh vpsguard@YOUR_SERVER_IP -p 22
sudo whoami
```

Only close the root session after confirming that the new user login works.

## License

MIT
