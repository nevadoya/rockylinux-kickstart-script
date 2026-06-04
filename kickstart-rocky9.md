# rockylinux-kickstart-script
Anfitrion -> Debian 13, Invitado (guess) -> Rocky Linux 9.7 (virtual machine) on kvm/qemu
Server Debian 13 is running old PC, firmware BIOS Legacy, this my .cfg file that works correctly.

```bash
# Kickstart - Servidor Web ws2.intranet.edu, rockylinux9
text
cdrom

# REPOSITORIOS CORRECTOS (desde internet)
repo --name="BaseOS" --baseurl="https://dl.rockylinux.org/pub/rocky/9/BaseOS/x86_64/os/"
repo --name="AppStream" --baseurl="https://dl.rockylinux.org/pub/rocky/9/AppStream/x86_64/os/"

# Localizacion
lang es_PE.UTF-8
keyboard --xlayouts='es'
timezone America/Lima --utc

# Red con Ip fija
network --bootproto=static --ip=192.168.1.6 --netmask=255.255.255.0 --gateway=192.168.1.1 --nameserver=9.9.9.9,1.1.1.1 --hostname=ws2 --activate

# Usuarios
rootpw --plaintext root_password
user --name=carla --plaintext --password=carla_password --groups=wheel

bootloader --location=mbr
clearpart --all --initlabel
autopart
reboot

%packages
@^minimal-environment
glibc-langpack-es
bash-completion
httpd
php
php-mbstring
php-intl
php-gd
php-fpm
vim-enhanced
%end

# Post-Instalation
%post --log=/root/kickstart-post.log

# Localization
localectl set-locale LANG=es_PE.UTF-8
localectl set-keymap es
echo -e "192.168.1.6\tws2.intranet.edu\tws2" >> /etc/hosts

# Set up services
# Enable on boot time
systemctl enable httpd php-fpm firewalld

# Firewall on chroot (kickstart method)
firewall-offline-cmd --add-service=http --zone=public

echo "=== Set up firewalld ===" > /root/firewall-config.txt
firewall-offline-cmd --list-all >> /root/firewall-config.txt

# Test php page
echo "<?php phpinfo(); ?>" > /var/www/html/info.php
%end
```

Good
