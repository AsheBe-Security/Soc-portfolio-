# Vulnerability scanning (Reconnaissance)
### nmap  scanning on Ubuntu server using Kali

1. prepare the ubuntu server
   
- sudo apt update && sudo apt upgrade -y
- sudo apt install net-tools curl wget unzip -y
2. install apache for the web service port 80

- sudo apt install apache2 -y
- sudo systemctl enable apache2
- sudo systemctl start apache2
  
**testing** 
- curl http://localhost
  
3. enable HTTPS service (port 443)
- sudo a2enmod ssl
- sudo a2ensite default-ssl
- sudo systemctl reload apache2
  
**nmap scanning** 
- nmap -p 80,443 -sV <target-ip>

4. FTP service (port 21)
- sudo apt install vsftpd -y
  
**Edit its configuration file**
- sudo nano /etc/vsftpd.conf
  
**Make a change to the followings** 
- anonymous_enable=YES
- write_enable=YES
  
**Restart the ftp service on the server** 
- sudo systemctl restart vsftpd
  
**nmap command**
- nmap -p 21 --script ftp-anon,ftp-bounce <target-ip>

5. Run ssh service on port 22
- sudo apt install ssh
  
**Change the config file**
- sudo nano /etc/ssh/sshd_config
  
**Make this change**
- PermitRootLogin yes
- PasswordAuthentication yes
  
**Restart the ssh**
- sudo systemctl restart ssh
  
**Run the nmap scan** 
- nmap -p 22 --script ssh-auth-methods,ssh-hostkey <target-ip>

6. MSTP Mail server on port 25
- sudo apt install postfix -y
   - choose 
     - Internet Site
       
**Run the nmap**
- nmap -p 25 --script smtp-commands,smtp-open-relay <target-ip>
7. Database services
  
MySQL port 3306
- sudo apt install mysql-server -y
  
**Allow remote access**
- sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
  
**Make a change**
- bind-address = 0.0.0.0
  
**Restart**
sudo systemctl restart mysql

**Run the nmap**
- nmap -p 3306 --script mysql-info,mysql-enum <target-ip>

8. SMB port 445
sudo apt install samba -y

**Create shares**
- sudo mkdir /srv/smbshare
- sudo chmod 777 /srv/smbshare
  
**Edit config**
- sudo nano /etc/samba/smb.conf
 
**Add the following**
[public]
- path = /srv/smbshare
- browsable = yes
- writable = yes
- guest ok = yes
  
**Restart the database**
- sudo systemctl restart smbd
  
**Run the nmap**
- nmap -p 445 --script smb-os-discovery,smb-enum-shares <target-ip>


 


