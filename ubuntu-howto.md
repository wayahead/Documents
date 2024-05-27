# Ubuntu 24.04

## Remove `unattended-upgrades`

```
$ sudo systemctl stop unattended-upgrades.service
$ sudo apt remove unattended-upgrades
$ sudo apt purge unattended-upgrades
```

## Update APT Sources

```
$ sudo vi /etc/apt/sources.list.d/ubuntu.sources
Types: deb deb-src
URIs: http://cn.archive.ubuntu.com/ubuntu/
Suites: noble noble-updates noble-backports
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

Types: deb deb-src
URIs: http://security.ubuntu.com/ubuntu/
Suites: noble-security
Components: main restricted universe multiverse
Signed-By: /usr/share/keyrings/ubuntu-archive-keyring.gpg

$ sudo apt update
$ sudo apt list --upgradable
```

## Configure Timezone

```
$ timedatectl list-timezones
$ sudo timedatectl set-timezone Asia/Shanghai
```

## Configure Vim

```
$ sudo apt install vim
$ vim ~/.vimrc
:set tabstop=2 shiftwidth=2 expandtab
:set nocompatible
```

## Configure Nano

```
$ vim ~/.nanorc
set tabsize 2
set tabstospaces
```

## Configure Git

```
$ git config --global user.email "kun.liu@outlook.com"
$ git config --global user.name "kun.liu"
```

## Configure fsck on boot

```
$ sudo nano /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash fsck.mode=force fsck.repair=yes"
$ sudo update-grub
```

## Configure Networks

### List Network Interfaces

```
$ sudo networkctl
$ sudo networkctl status -a
$ sudo ip address
```

### Show Listening Ports

```
$ sudo ss -tunelp
$ sudo ss -nlt
```

### Check Network Status

```
$ sudo networkctl status -a
```

### Check Connection Status

```
$ sudo ss -aunp
$ sudo ss -ltpan
$ sudo ss -tunelp
```

```
$ sudo ss -tlp
$ sudo ss -tulpn
```

```
$ sudo ss -s
$ sudo ss -a -o
```

### Use NetPlan

#### Static IP address in NetPlan

```
$ sudo netplan generate 
$ ip link
$ sudo nano /etc/netplan/01-netcfg.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    ens3:
      dhcp4: no
      addresses: [192.168.1.222/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
$ sudo netplan apply
$ sudo netplan --debug apply
$ ip addr show dev ens3
$ resolvectl status | grep "DNS Server" -A2
```

* Baoding IDC

```
$ sudo vi /etc/netplan/00-installer-config.yaml
network:
  ethernets:
    eno1:
      addresses:
      - 111.63.48.41/27
      gateway4: 111.63.48.33
      nameservers:
        addresses:
        - 111.11.11.1
        - 111.11.1.1
  version: 2
```

* Lianyungang IDC

```
IP Addresses: 223.111.197.130-223.111.197.133
Subnetwork Mask: 255.255.255.248 (/29)
Subnetwork: 223.111.197.128/29
Gateway: 223.111.197.129
DNS: 112.4.0.55
```

```
$ sudo vi /etc/netplan/00-installer-config.yaml
# This is the network config written by 'subiquity'
network:
  ethernets:
    eno1:
      addresses:
      - 223.111.197.130/29
      gateway4: 223.111.197.129
      nameservers:
        addresses:
        - 112.4.0.55
        - 114.114.114.114
  version: 2
```

#### Dynamic IP address in NetPlan

```
$ sudo netplan generate 
$ ip link
$ sudo nano /etc/netplan/01-netcfg.yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp0s3:
      dhcp4: true
      dhcp6: true
    enp0s8:
      dhcp4: true
      dhcp6: true
$ sudo netplan apply
$ sudo netplan --debug apply
```

### Disable IPv6

#### Disable IPv6 in `/etc/default/grub`

```
$ sudo vi /etc/default/grub
GRUB_CMDLINE_LINUX="maybe-ubiquity transparent_hugepage=never ipv6.disable=1"
GRUB_CMDLINE_LINUX_DEFAULT="maybe-ubiquity transparent_hugepage=never ipv6.disable=1"
$ sudo update-grub
```

#### Disable IPv6 via `sysctl`

```
$ sudo nano /etc/sysctl.d/60-custom.conf
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
net.ipv6.conf.lo.disable_ipv6 = 1
net.ipv6.conf.wlan0.disable_ipv6 = 1
$ sudo sysctl -p
$ sudo systemctl restart procps
$ cat /proc/sys/net/ipv6/conf/all/disable_ipv6
```

#### Disable IPv6 in Netplan

```
$ sudo vi /etc/netplan/10-wifi.yaml
network:
  ethernets:
    wlan0:
      link-local: [ipv4]
      dhcp4: no
      address: [192.168.1.2/24]
      gateway: 192.168.1.1
      nameservers:
        adresses: [127.0.0.1, 10.10.60.1]
  version: 2
$ sudo netplan apply
```

#### Disable IPv6 in NetworkManager

```
"IPv6 Tab" => "Disable"
```

```
$ sudo systemctl restart NetworkManager
```

#### Disable IPv6 in APT

```
$ sudo nano /etc/apt/apt.conf.d/99force-ipv4
Acquire::ForceIPv4 "true"
```

## Configure SSH (Remote-Access)

### Install SSH

```
$ dpkg --get-selections|grep ssh
$ sudo apt install ssh
```

```
$ dpkg --get-selections|grep ssh
$ sudo apt install openssh-server openssh-client
```

```
$ sudo vi /etc/ssh/sshd_config
Port 12085
PermitRootLogin no
AddressFamily inet
$ sudo systemctl restart sshd.service
```

### Generate RSA Public/Private Keys at Client Side

```
$ ssh-keygen -b 4096
$ ssh-keygen -p
$ ssh-keygen -l
```

### Sync Public Key to Server Side

* If you have ssh-copy-id at hand

```
$ ssh-copy-id -p port wayahead@server
$ ssh -p port -i <private_key.file> wayahead@server
$ sftp -i <private_key.file> wayahead@server
```

* When there is no ssh-copy-id at hand

```
$ cat ~/.ssh/id_rsa.pub | ssh -p port wayahead@server "mkdir -p ~/.ssh && cat >> ~/.ssh/authorized_keys"
$ ssh -p port -i <private_key.file> wayahead@server
$ sftp -i <private_key.file> wayahead@server
```

### Disable Password Authentication at Server Side

```
$ sudo vi /etc/ssh/sshd_config
PubkeyAuthentication yes
PasswordAuthentication no
AuthorizedKeysFile     .ssh/authorized_keys .ssh/authorized_keys2

AllowGroups ssh-users sftp-users

Match Group sftp-users
ChrootDirectory /home/%u
PasswordAuthentication no
PermitTunnel no
AllowAgentForwarding no
AllowTcpForwarding no
X11Forwarding no
ForceCommand internal-sftp -d %u -u 0111

$ sudo systemctl restart sshd.service
```

## Enable Fail2ban

```
$ sudo apt install fail2ban
$ sudo systemctl status fail2ban.service
$ vi /etc/fail2ban/jail.local
[DEFAULT]
destemail = your@email.here
sendername = Fail2Ban

[sshd]
enabled = true
port = 60791
filter = sshd
logpath = /var/log/auth.log
bantime = 110w

[sshd-ddos]
enabled = true
port = 60791
filter = sshd
logpath = /var/log/auth.log
bantime = 110w

[recidive]
enabled = true
maxretry = 3
bantime = 110w
findtime = 1d
$ sudo systemctl restart fail2ban.service
$ sudo systemctl status fail2ban.service
$ sudo vi /var/log/fail2ban.log
```

## Configure User

```
$ ssh root@host (*G)

> adduser wayahead (aG)
> adduser wayahead sudo
> useradd wayahead && passwd wayahead
> usermod -aG wheel wayahead
```

## Configure Firewall/UFW

```
$ sudo vi /etc/default/ufw
IPV6=no
$ sudo ufw reload
```

```
$ sudo ufw status
$ sudo ufw status numbered
$ sudo ufw enable
$ sudo ufw allow 12085/tcp
$ sudo ufw allow 80/tcp
$ sudo ufw allow 443/tcp
$ sudo ufw allow from <ip_address> to any port 16001 proto tcp
$ sudo ufw allow from <ip_address> to any port 60791
$ sudo ufw delete allow 500/tcp 500/udp
$ sudo ufw delete allow from any port 68 to any port 67 proto udp
$ sudo ufw deny from <ip> port 22
```

## Configure Limit

```
$ sudo vi /etc/security/limits.conf
*    soft    nproc    65535
*    hard    nproc    65535
*    soft    nofile   65535
*    hard    nofile   65535
root    soft    nproc    65535
root    hard    nproc    65535
root    soft    nofile   65535
root    hard    nofile    65535
```

```
$ sudo vi /etc/pam.d/common-session
session required    pam_limits.so
```

```
$ sudo ulimit -n
$ sudo vi /etc/sysctl.conf
fs.file-max = 65535
net.core.somaxconn = 65535
vm.overcommit_memory = 1
net.ipv4.tcp_timestamps = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_max_tw_buckets = 262144
net.ipv4.tcp_fin_timeout = 30
net.ipv4.icmp_echo_ignore_all = 1
$ sudo sysctl -p
```

## Disable THP (Transparent Huge Pages)

### Configure `/etc/default/grub`

```
$ sudo vi /etc/default/grub
GRUB_CMDLINE_LINUX="transparent_hugepage=never"
GRUB_CMDLINE_LINUX_DEFAULT="transparent_hugepage=never"
$ sudo update-grub
```

```
$ sudo vi /etc/default/grub
GRUB_CMDLINE_LINUX="maybe-ubiquity transparent_hugepage=never"
GRUB_CMDLINE_LINUX_DEFAULT="maybe-ubiquity transparent_hugepage=never"
$ sudo update-grub
```

```
$ sudo vi /etc/default/grub
GRUB_CMDLINE_LINUX="maybe-ubiquity transparent_hugepage=never ipv6.disable=1"
GRUB_CMDLINE_LINUX_DEFAULT="maybe-ubiquity transparent_hugepage=never ipv6.disable=1"
$ sudo update-grub
```

### Configure `/etc/rc.local`

```
$ sudo vi /etc/rc.local
echo never > /sys/kernel/mm/transparent_hugepage/enabled
echo never > /sys/kernel/mm/transparent_hugepage/defrag
exit 0

$ sudo reboot
```

* other option `/etc/rc.local`

```
$ sudo vi /etc/rc.local
if test -f /sys/kernel/mm/redhat_transparent_hugepage/enabled; then
  echo never > /sys/kernel/mm/redhat_transparent_hugepage/enabled
fi
if test -f /sys/kernel/mm/redhat_transparent_hugepage/defrag; then
  echo never > /sys/kernel/mm/redhat_transparent_hugepage/defrag
fi
$ sudo reboot
```

```
$ sudo grep -i HugePages_Total /proc/meminfo 
$ sudo cat /proc/sys/vm/nr_hugepages
```

### Disable THP Temporarily

```
$ sudo cat /sys/kernel/mm/transparent_hugepage/enabled
$ sudo cat /sys/kernel/mm/transparent_hugepage/defrag
```

```
$ sudo su
# echo never > /sys/kernel/mm/transparent_hugepage/enabled
# echo never > /sys/kernel/mm/transparent_hugepage/defrag
```

### verify THP

```
root@netcore:~# cat /sys/kernel/mm/transparent_hugepage/enabled
always madvise [never]
root@netcore:~# cat /sys/kernel/mm/transparent_hugepage/defrag
always defer defer+madvise [madvise] never
```

## Disable IPv6 in `/etc/default/grub`

```
$ sudo vi /etc/default/grub
GRUB_CMDLINE_LINUX="ipv6.disable=1"
GRUB_CMDLINE_LINUX_DEFAULT="ipv6.disable=1"
$ sudo update-grub
```

```
$ sudo vi /etc/default/grub
GRUB_CMDLINE_LINUX_DEFAULT="maybe-ubiquity transparent_hugepage=never ipv6.disable=1"
GRUB_CMDLINE_LINUX="maybe-ubiquity transparent_hugepage=never ipv6.disable=1"
$ sudo update-grub
```

## Configure SFTP

### Install

```
$ dpkg --get-selections | grep ssh
$ sudo apt install openssh-sftp-server openssh-server openssh-client
```

```
$ sudo vi /etc/ssh/sshd_config
Port 12085
PermitRootLogin no
AddressFamily inet
```

### Create Group and User

```
$ sudo addgroup sftp-users

$ sudo adduser sftpuser (WqA1yT2z)
$ sudo usermod -G sftp-users -s /bin/false sftpuser

$ sudo adduser sunshine (WqA1yT2z)
$ sudo usermod -G sftp-users -s /bin/false sunshine

$ sudo addgroup ssh-users
$ sudo usermod -a -G ssh-users wayahead
```

```
$ sudo deluser sunshine
$ sudo delgroup ssh-users
```

### Create Directory for File Transfers

```
$ sudo chown root:root /home/sftpuser
$ sudo chmod 0755 /home/sftpuser

$ sudo mkdir -p /home/sftpuser/requests
$ sudo chown sftpuser:wayahead /home/sftpuser/requests
$ sudo chmod 0770 /home/sftpuser/requests

$ sudo mkdir -p /home/sftpuser/results
$ sudo chown sftpuser:wayahead /home/sftpuser/results
$ sudo chmod 0770 /home/sftpuser/results
```

```
$ sudo chown root:root /home/sunshine
$ sudo chmod 0755 /home/sunshine

$ sudo mkdir -p /home/sunshine/requests
$ sudo chown sunshine:wayahead /home/sunshine/requests
$ sudo chmod 0770 /home/sunshine/requests

$ sudo mkdir -p /home/sunshine/requests/receipts
$ sudo chown sunshine:wayahead /home/sunshine/requests/receipts
$ sudo chmod 0770 /home/sunshine/requests/receipts

$ sudo mkdir -p /home/sunshine/results
$ sudo chown sunshine:wayahead /home/sunshine/results
$ sudo chmod 0770 /home/sunshine/results

$ sudo mkdir -p /home/sunshine/results/receipts
$ sudo chown sunshine:wayahead /home/sunshine/results/receipts
$ sudo chmod 0770 /home/sunshine/results/receipts
```

### Restrict Access to Directory

```
$ sudo vi /etc/ssh/sshd_config
AllowGroups ssh-users sftp-users

Match Group sftp-users
ChrootDirectory /home/%u
# PasswordAuthentication no
PasswordAuthentication yes
PermitTunnel no
AllowAgentForwarding no
AllowTcpForwarding no
X11Forwarding no
ForceCommand internal-sftp -d %u -u 0111

$ sudo systemctl restart sshd.service
```

## Use UFW

### Tips

```
$ sudo ufw deny from <ip-address> to any
$ sudo ufw status numbered
$ sudo ufw deny from <ip-address> to any port <port>
$ sudo ufw deny proto <tcp|udp> from <ip-address> to any port <port>
$ sudo ufw deny proto tcp from <ip-address>/255.255.255.9 to any
$ sudo ufw deny proto tcp from <ip-address>/255.255.0.0 to any
$ sudo ufw deny proto tcp from <ip-address>/255.255.255.9 to any port <port>
$ sudo ufw deny proto tcp from <ip-address>/255.255.0.0 to any port <port>
```

### Deny IP

```
$ sudo ufw deny from <ip-address> to any
```

## Disable Ping/ICMP

```
$ sudo vi /etc/sysctl.conf
net.ipv4.icmp_echo_ignore_all = 1
$ sudo sysctl -p
```

## Disable IPv6

```
$ sudo vi /etc/sysctl.conf
net.ipv6.conf.all.disable_ipv6 = 1
net.ipv6.conf.default.disable_ipv6 = 1
$ sudo sysctl -p
```

## Check SSL/TLS Port

```
$ openssl s_client -connect <host>:<port>
```

## Configure Archive Mirrors

```
http://mirrors.tuna.tsinghua.edu.cn/ubuntu/
http://mirrors.sohu.com/ubuntu/
http://archive.ubuntu.com/ubuntu/
```
