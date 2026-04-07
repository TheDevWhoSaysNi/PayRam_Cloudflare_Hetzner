# Setting up PayRam using Cloudflare and Hetzner

PayRam is a free tool to accept crypto payments. But the Documentation is not the same for handling every type of install on every type of server. So I made this to help anyone who wants to use it with a free Cloudflare account for proxy, and a cheap VPS from Hetzner.

## Description

This assumes you already have an account with Cloudflare, and with Hetzner. Account setup is free, and is easy to do with any LLM.

## Getting Started

### Prerequisite Documentation

* Refer to [official documentation](https://docs.payram.com/) which can change at times.

### Hetzner Prerequisites
  
* OPTIONAL: Security -> SSH Keys -> Add your public key if you have one.
* [Hetzner Console](https://console.hetzner.com/): Create a CX23 with an IPV4 option. 40GB default is enough if you are running a small business instance. OS must be Ubuntu 22.04.
* [Hetzner Console](https://console.hetzner.com/): Firewalls -> Create Firewall -> Any IPV4/IPV6, TCP for ports 22, 80, 443, 8080, 8443.
  OPTIONAL: You can restrict to certain IPs for port 22 for extra security.
  IMPORTANT: You do NOT need to open port 5432.
* [Hetzner Console](https://console.hetzner.com/): Servers -> Choose your server -> Rescue -> Reset root password -> Copy new password and save to password manager.
* [Hetzner Console](https://console.hetzner.com/): Open CLI console on Hetzner UI (top right of screen) -> log in as root, paste password -> Set up SSH login (you might need to manually type it in):
  ```bash
  nano /etc/ssh/sshd_config
  ```
  Find #PermitRootLogin and remove the # and change whatever is after it to "yes". That line should turn from blue to white and show "PermitRootLogin yes".
  Find #PasswordAuthentication and remove the # and add "yes". That line should turn from blue to white and show "PasswordAuthentication yes".
  CTRL+O, Enter, CTRL+X
  Restart SSH:
  ```bash
  systemctl restart ssh
  ```
* On your Terminal or Powershell: SSH into your VPS:
  ```bash
  ssh root@XXX.XXX.XXX.XXX
  ```
* OPTIONAL if using SSH key, you will need to manually fix it in your VPS:
  ```bash
  cat << 'EOF' > ~/.ssh/authorized_keys
  [PASTE YOUR PUBLIC SSH KEY HERE]
  EOF
  ```
  ```bash
  chmod 700 ~/.ssh
  chmod 600 ~/.ssh/authorized_keys
  ```
  ```bash
  exit
  ```
  ```bash
  ssh root@XXX.XXX.XXX.XXX
  ```
  You should be able to log in without requiring a password.

### Cloudflare Prerequisites

* Create an A record on [Cloudflare](https://dash.cloudflare.com/) with subdomain of your choice "i.e. pay" and point to your new IPV4 address. Turn proxy (orange cloud) ON.
* SSL/TLS -> Overview -> Flexible OR Full OR Full (Strict)
* SSL/TLS - Edge Certificates -> Always use HTTPS = ON
* Describe any prerequisites, libraries, OS version, etc., needed before installing program.

### Installing

On your Terminal or PowerShell...
* Ubuntu 22.04 does not allow arguments for the default command, so it needs to be changed:
  ```bash
  curl -fsSL https://raw.githubusercontent.com/PayRam/payram-scripts/main/setup_payram.sh -o setup.sh
  ```
    ```bash
  cchmod +x setup.sh
  ```
  ```bash
  ./setup.sh --mainnet
  ```
* Choose if you want the SQL server on the same image. For small instances, choose OPTION #2.
* Choose OPTION #3 for no SSL. Then hit "y". Then continue the install until complete.
* Set up a Let's Encrypt certificate after install is complete.
  ```bash
  bash <(curl -fsSL https://payram.com/setup_payram.sh)
  ```
  Choose OPTION 5 to update the SSL configuration, then OPTION 1 to generate SSL certificate.
* IMPORTANT: while waiting for the SSL certificate to sync with Cloudflare, don't forget to collect your AES-256 key:
  ```bash
  ls -R /root/.payraminfo/aes/
  ```
  Copy the output and save it in a password manager or safe location.

### Confirming Success

* Go to your new page:
  ```bash
  https://pay.YOURDOMAIN.com/signup
  ```
* Enter your root email.
* Create your password. The docs do not say it but it is a 72 character max. So if you use complex password managers do not go more than 72 chars or you will get "Internal Server Error".
* THE MOMENT OF TRUTH, it should go straight to creating a project name, and then go to your dashboard.
* CONDITIONAL: if you get a CORS error, copying what is in F12 Console is very likely futile because LLMS are useless on this. It is best to go to their Telegram chat page and give as much context as you can. But as of April 7, 2026, this is the exact method that worked for me.

## Help

* Official Telegram Chat [link](https://t.me/PayRamChat).
* Quick setup [link](https://docs.payram.com/deployment-guide/quick-setup).
* Advanced setup [link](https://docs.payram.com/deployment-guide/advanced-setup).
* Onboarding [link](https://docs.payram.com/onboarding-guide/introduction).

## Authors

TheDevWhoSaysNi
[ryan@suncoastservers.com](mailto:ryan@suncoastservers.com)
[Telegram](https://t.me/thedevwhosaysni)

## License

MIT License, hope this helps.

## Ni

Ni.
