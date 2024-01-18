### 1 Kerberos Server Configuration
### 1.1 Install and configure a Kerberos server.

#### 1.1.1 Configure hostname

```shell
sudo nano /etc/hosts
hostnamectl --static set-hostname client.local.tn  
````
#### 1.1.2 Install kerberos
Configure the realm in the appropriate configuration files to match the server's krb5.conf file. Ensure consistency by setting the same realm and administrators on the server.
````shell
sudo apt-get update
sudo apt install krb5-user
````
![](https://drive.google.com/uc?id=1DLVXGKpVpcu1qIzTCm3Sg7-bgd41Qo2h)
![](https://drive.google.com/uc?id=1iWm_pWonpmkYP1W9-oSile5OOQfY8NIG)
### 1.1.3 Install openSSH 

````shell
  sudo apt update
  sudo apt install openssh-server
  #Uncomment the lines of GSSAPIAuthentication and GSSAPICleanUpCreadentials and set them to yes
  sudo nano /etc/ssh/sshd_config
  sudo nano /etc/ssh/ssh_config
  sudo systemctl restart sshd
  sudo systemctl status sshd
````
![](https://drive.google.com/uc?id=1m-ssijIBWt8TzqRGluHBVzDPHnaqEr0w)
![](https://drive.google.com/uc?id=1iQTt_PhwHwS70RTJNndt7MU2b3vMm5QK)
### 2.2 SSH Authentication

#### 2.2.1 Create a new user account && Log in using it
````shell  
adduser utilisateur
su -l utilisateur 
````
![create new user](https://drive.google.com/uc?id=17QAirmxXo8_Gx4-ZvHGu8BbjBIUtO1JL)

#### 2.2.2  Authenticate using TGT
The service principal and key in the keytab file are specific to the SSH service on the mentioned server (kdc.SERVER.tn).  
When a user obtains a Ticket-Granting Ticket (TGT) during the authentication process, that TGT allows the user to request service tickets for services within the Kerberos realm, including the SSH service on kdc.SERVER.tn
````shell 
kinit
klist
ssh kdc.server.tn
w 
````
![authenticating](https://drive.google.com/uc?id=1xTiUP04ZrbBOYzOD89JemdyjdGe3kgTa) 
![](https://drive.google.com/uc?id=1xOforgKKAFdr7WVQlX96HJ1ZfwTYt5O2)
#### 2.2.3 Copy file :
securely copies the file named "file" from the remote host kdc.server.tn to the current directory on the local machine.
````shell
scp kdc.server.tn:file .
````
![](https://drive.google.com/uc?id=10TP-3xb9u3cy-S9R78vtBzlfpeAbTg4v)

[Go back](./kerberos.md)