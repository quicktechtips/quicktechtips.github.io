---
layout: default
title: Lockdown FTP web users
nav_order: 3
---
# How to Lock Down FTP Web User Accounts

Stick to SSH/SFTP/FTPS if you can. However, if FTP is the only way, then at least limit the potential damage.
This procedure can be used to secure FTP services for webserver  user accounts (Red Hat based operating system).

## Steps

1. Install vsftpd: `dnf install vsftpd`
2. Start and enable vsftpd: `systemctl start vsftpd && systemctl enable vsftpd`
3. Create FTP user: `useradd username`
4. Set user password: `passwd username`
  - Make this unique for this server
5. Prevent FTP users from logging in via SSH:
  - `echo "DenyUsers username1 username2" >> /etc/ssh/sshd_config.d/51-deny-ftp-users.conf`
6. Restart sshd: `systemctl restart sshd`
7. Chroot local users in vsftpd:
  - `vim /etc/vsftpd/vsftpd.conf`
  - Uncomment the line: `chroot_local_user=YES`
  - [Optional] Change the FTP banner to empty string if desired as well
  - [Optional] Change `listen=NO` to `listen=YES` and comment out the line `listen_ipv6=YES`
8. Restart vsftpd: `systemctl restart vsftpd`
9. Make home directory for FTP user non-writable: `chmod a-w /home/username`
  - **Note**: cannot allow FTP user to have write ability to root directory in chroot--this would allow chroot escape and code execution
10. Make directories as needed for FTP user: `mkdir /home/username/www`
11. Bind mount the directory to the proper location:
  - `mount --bind /var/www/html/specific/directory /home/username/www`
12. Make the bind mount survive reboots:
  - Open `/etc/fstab` and add the line
  - `/var/www/html/specific/directory	/home/username/www	none	bind	0	0`
13. Lockdown firewall to the IP addresses needed:
  - `firewall-cmd --zone=ftp-access --add-source=10.0.0.0/24 --permanent`
  - `firewall-cmd --zone=ftp-access --add-port=21/tcp --permanent`
  - Note: additional ports will need to be opened if using passive FTP instead of active
14. Reload firewalld: `systemctl reload firewalld`
15. Test connection and uploads to the server. 

## Notes

- Run `journalctl -xe` to look for SELinux errors if issues arise.
- If FTP is needed in passive mode, then you must configure port ranges in `/etc/vsftpd/vsftpd.conf` and set them in the firewall.
  - `printf "pasv_enable=YES\npasv_max_port=10100\npasv_min_port=10090\n" | sudo tee -a /etc/vsftpd/vsftpd.conf > /dev/null`
  - `sudo firewall-cmd --zone=ftp-access --add-port=10090-10100/tcp --permanent`

