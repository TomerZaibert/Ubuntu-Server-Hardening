# Web Server Security Hardening

## Objective
 The objective gain hands-on experience securing a web server by implementing real-world hardening techniques. This project involved setting up an Ubuntu-based LAMP stack (Linux, Apache, MySQL, PHP) and applying security measures to reduce potential attack surfaces.The goal was to improve my understanding of server security best practices and demonstrate my ability to configure and protect a web server from common threats.

## Write-Up

### Installing Ubuntu Server
   To set up the secure web server, I first installed Ubuntu Server. This provided a stable Linux environment for hosting the LAMP stack.

   ![screenshot1 - ubuntu installed](https://github.com/user-attachments/assets/1781f106-146c-47ce-a974-55707edfb341)

### Updating System Packages
 After installation, I needed to make sure all system packages were up to date to ensure the latest security patches and stability.
   
Commands used:
sudo apt update && sudo apt upgrade -y

  ![screenshot2 - ubuntu update](https://github.com/user-attachments/assets/ad74e540-299f-4b75-8fef-e7fbff3bfd2b)

### Checking IP Address
i needed to check my IP to make sure it was correctly configured and accessible, and for later use.

commands used:
ip a | grep inet

![screenshot3 - ip check](https://github.com/user-attachments/assets/980acd66-92ad-4ecd-b242-20eb1c2ad896)

### Enabling SSH Server
activation of the SSH server to allow secore remote access to the server.

commands used:
sudo systemctl start ssh,
sudo systemctl enable ssh,
sudo systemctl status ssh,

![screenshot4 - activating ssh server](https://github.com/user-attachments/assets/92c9c2bb-7fb3-4b41-b52d-36a74bd13b53)

### Installing Apache
Apache was installed as the web server and configured to start on boot.

commands used: sudo apt install apache2 -y, 
sudo systemctl enable apache2, 
sudo systemctl start apache2, 
sudo systemctl status apache2, 

![screenshot5 - apache install and active](https://github.com/user-attachments/assets/040f6b94-101f-49bc-8ae2-f36c68990631)

### Installing and Securing MariaDB
MariaDB was installed as the database server and secured using the mysql_secure_installation script.

Commands used:
sudo apt install mariadb-server -y, 
sudo mysql_secure_installation, 

![screenshot6 - mariadb installed and status chcked](https://github.com/user-attachments/assets/52f22928-2cef-405e-b612-73bdcadd6dff)
![screenshot7 - mariadb secured](https://github.com/user-attachments/assets/041defcf-e1fb-4d47-bece-85a2ae5e1b5f)


### Installing and Configuring PHP
I installed PHP and tested it by creating phpinfo file.

commands used:
sudo apt install php libapache2-mod-php php-mysql -y, 
php -v

![screenshot8 - php installed and working](https://github.com/user-attachments/assets/89fa267c-5213-4b39-bf77-4594266ce6e4)

### Verifying PHP Configuration
created a test PHP in /var/www/html/info.php to confirm PHP was running properly.

Commands used:
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php

![screenshot9 - php info file](https://github.com/user-attachments/assets/047b4841-0cf4-4e24-a33f-52bcaf9ab815)

after we verified PHP was running properly i deleted the test file to prevent attackers from seeing server details using the command: sudo rm /var/www/html/info.php.

### Hardening Apache Configuration
Modification to the Apache configuration file to apply security best practices.


![screenshot10 - apache config before change](https://github.com/user-attachments/assets/97a7ff4a-2b48-44d2-837e-d976dfa8bd16)

![screenshot11 - apache config after change](https://github.com/user-attachments/assets/84c4ad61-e389-4cb6-9509-e756984f4529)

<Directory /var/www/>

    Options Indexes FollowSymLink
    
was changed to:

<Directory /var/www/>
    Options -Indexes +FollowSymLinks

I also modified Apache's security settings to minimize information exposure.

![screenshot12 - security conf before change](https://github.com/user-attachments/assets/6b01c384-20da-487e-a0b4-3285b002b9fd)

![screenshot13 - security conf after change](https://github.com/user-attachments/assets/091dbeb7-3c13-4ab7-8a88-7a1548e1488d)

### Configuring MYSQL Security

modified the MySQL configuration file to restrict access and improve security.

![screenshot14 - mysql conf](https://github.com/user-attachments/assets/e675ea01-011e-40cc-b553-adefe2430ed3)

verified that MySQL was configured to only listen on localhost to prevent remote access.

![screenshot15 - checking mysql bind address](https://github.com/user-attachments/assets/a7998a04-e316-4612-aa39-ce7d5d42b644)

tried checking the ports the MYSQL was listening to.
faced some problems getting the expected resulsts from MySQL. after further checks i decided to use a different command to ensure it was listening to the right ports and with the correct IP.

![screenshot16 - ports check listening](https://github.com/user-attachments/assets/acd03f3f-0b60-4881-84c7-94a22afb21c2)

### Updating UFW Rules

configured UFW (Uncomplicated Firewall) to allow necessary traffic and secure the server.

Commands used:
sudo ufw allow OpenSSH,
sudo ufw allow 'Apache Full',
sudo ufw allow 3306/tcp,
sudo ufw enable,
sudo ufw status verbose,

![screenshot17 - update ufw rules](https://github.com/user-attachments/assets/ca0b2d30-dfc4-4c9f-97bf-e8da13354e67)

### Securing SSH Configuration

I updated the SSH settings to prevent root login and ensure secure access.

commands used:
sudo nano /etc/ssh/sshd_config

modified #PermitRootLogin prohibit-password to PermitRootLogin no.

![screenshot18 - ssh config file](https://github.com/user-attachments/assets/f2e08209-91b3-455f-912d-5f64f3011bb3)

### Enabling Automatic Security Updates

enabled automatic security updates to ensure continuous protection against vulnerabilities.

Command used:
sudo systemctl status unattended-upgrades.

I followed the programm's prompts to enable automatic security.

![screenshot19 - auto sec updates enabled](https://github.com/user-attachments/assets/f6db1153-1662-43c0-9fe6-9df331bdb436)

### Verifying Firewall and Open Ports

I verified open ports and firewall rules using the command 'sudo ss -tulnp' to ensure only necessary services were accessible.

![screenshot20 - validating firewall and open ports](https://github.com/user-attachments/assets/a0a86502-83a3-4311-a264-5b17ef132583)

### SSH Root Access Restrictions

I tried testing the SSH root login to confirm it was disabled.

I wasn't sure whether the restriction worked or not, so I decided to check if there was a password on the root user, even though that by default it shouldn't have. 
Seeing there was no password meant that the SSH root access was restricted as I intended.

![screenshot25 - rootpassword check](https://github.com/user-attachments/assets/f478b218-3abf-4585-91d7-f9003ead25e9)

the * after in the second field indicates there is no password.


![screenshot21- root access doesnt work](https://github.com/user-attachments/assets/79738db0-5676-4401-b66a-b98afc27c124)

![screenshot22 - root access denied](https://github.com/user-attachments/assets/69c84bbc-dd45-4c67-82f6-19220965fc19)

### Firewall Rule Confirmation

check UFW rules and wasn't happy with the rules, there were security risks:

OpenSSH (port 22/tcp) was accessible from anywhere, which is a security risk.

MySQL (port 3306/tcp) was also open to anywhere, which could allow external attacks.

Restricted SSH to my specific IP (213.57.115.198).
Restricted MySQL to localhost only (127.0.0.1).

![screenshot23 - ufw verbose check](https://github.com/user-attachments/assets/8b199783-6ef7-449c-b879-0653aa3312c6)

![screenshot24 - ufw verbose after fixes](https://github.com/user-attachments/assets/19bdc3bc-2eab-4ffc-9f45-cc4a6b99b8c7)

These final changes made sure the server was secure and ready to deploy.


## Conclusion

Doing this project gave me practical experience in securing a web server, from initial setup to applying security hardening techniques. I enjoyed figuring out the right settings and finding the best practices for making a secure web server while cross-referecing sources. I configured SSH to prevent unauthorized access, fine-tuned firewall rules to allow only necessary traffic, and hardened Apache and MySQL to minimize exposure to vulnerabilities. This small project reinforced my Linux administration skills and strengthened my understanding of cybersecurity fundamentals in a real-world environment.






















