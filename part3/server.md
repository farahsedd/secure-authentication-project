### 1 Kerberos Server Configuration
### 1.1 Install and configure a Kerberos server.
#### 1.1.1 Configure hostname
```shell
sudo nano /etc/hosts
hostnamectl --static set-hostname kdc.sever.tn  
````
#### 1.1.2 Install kerberos
```shell
sudo apt-get update
sudo apt install krb5-kdc krb5-admin-server krb5-config
````

#### 1.1.3 Edit Kerberos Configuration File
Edit the Kerberos configuration file (/etc/krb5.conf) to define the realm, KDC information, and global parameters.  
Edit kdc configuration file (/etc/kdc.conf)  to Specify KDC settings, policies, and encryption types as it configures the Kerberos Key Distribution Center (KDC).
```shell
nano etc/krb5.conf
```
![kerberos config ](https://drive.google.com/uc?id=1mz7qUFRnorpId6izc37X86KWUpvTtOV3)


```shell
nano etc/kdc.conf
```
![ ](https://drive.google.com/uc?id=1N3zZfF74xabl4n1Zw6ZqBctHTgs3IlSc)

#### 1.1.4 Initialize a new Kerberos realm
edit kdm5.acl file to define administrative access rules
```shell
sudo su
cd /etc/krb5kdc
/etc/krb5kdc# : krb5_newrealm
/etc/krb5kdc# nano kadm5.acl
````
![](https://drive.google.com/uc?id=1sTHkB6r8zqRxHMv48S6hAqK47P5ymFHZ)

### 1.2 Add principals and password policies for users.

#### 1.2.1 Add principal users
Use the kadmin.local utility to create principals for the KDC admin
```shell
kadmin.local:add_principal utilisateur
kadmin.local:get_principal utilisateur
kadmin.local:get principals
````
![add principals ](https://drive.google.com/uc?id=14ldOnxG-hu55ZP57OQdMCSBjghSenjMu)
![ ](https://drive.google.com/uc?id=1TxbNwM6RKU801TemQfFJt5cBfAQ1OyNA)


#### 1.2.2 Create Kerberos ticket-granting ticket (TGT)
TGT is created  during a user's login session to allowing them to access services without re-entering their password.
````shell  
kinit 
klist -kte kadm5.keytab
````

![ticket generating](https://drive.google.com/uc?id=1eF-4tgYkZxQWudmYfHKKV72MShNUgGWr)
![](https://drive.google.com/uc?id=1ThVI76fOqU273ZcY9l6Freav3oepD379)

#### 1.2.3 Add policies to each user
Policy sets security rules for aspects like passwords, ticket duration, and encryption settings for user and service principals.
we add a principal for the root user associated with the KDC server, specifying a key version of 1 and using the AES256 encryption type.
The key is then written to a keytab file (kadm5.keytab) for secure authentication,
````shell  
  addent -password -p root/kdc@SERVER.TN -k 1 -e aes256-cts-hmac-sha1-96
  wkt kadm5.keytab
  rkt kadm5.keytab
  l
````
![add policies](https://drive.google.com/uc?id=187BzGtxu9_aywQ7qiXh9NE9mM9oNB30z)
![](https://drive.google.com/uc?id=1sQMvIOX0l6Zy38xrbHnL5M11jAj4_IEW)
![](https://drive.google.com/uc?id=1eF-4tgYkZxQWudmYfHKKV72MShNUgGWr)


### 2 Authentication with SSH
### 2.1 SSH Configuration

#### 2.1.1 Install ssh && Edit sshd_config and ssh_config files
````shell  
apt install openssh-server
````
![install ssh](https://drive.google.com/uc?id=16PHX6iz28uoeYwWy-D_kdbjJnOdRIE3E)

Uncomment and set GSSAPIAuthentication and GSSAPICleanUpCreadentials to yes in SSH configuration files. Restart the SSH service.

![configure ssh files](https://drive.google.com/uc?id=16PHX6iz28uoeYwWy-D_kdbjJnOdRIE3E)

#### 2.1.2 Create a Principal Service for SSH

A principal, either user or service, is uniquely identified in the authentication system. Setting up a Kerberos KDC involves adding a host principal for the server with a random key in a keytab file. This keytab allows services to authenticate without passwords, storing keys from the initial TGT during principal creation.
````shell  
sudo kadmin.local
addprinc -randkey host/kdc.SERVER.tn
ktadd -k host/kdc.SERVER.tn
````
![create ssh service](https://drive.google.com/uc?id=16LVP-B5Obh1X1dHMKbOj1BYFeswuCq6X)

### 2.2 SSH Authentication

#### 2.2.1 Create a new user account && Log in using it
````shell  
adduser utilisateur
su -l utilisateur 
````
![create new user](https://drive.google.com/uc?id=17QAirmxXo8_Gx4-ZvHGu8BbjBIUtO1JL)

#### 2.2.1  Authenticate using TGT
The service principal and key in the keytab file are specific to the SSH service on the mentioned server (kdc.SERVER.tn).  
When a user obtains a Ticket-Granting Ticket (TGT) during the authentication process, that TGT allows the user to request service tickets for services within the Kerberos realm, including the SSH service on kdc.SERVER.tn
````shell 
kinit
klist
ssh kdc.server.tn
w 
````
![authenticating using TGT](https://drive.google.com/uc?id=1GuWaL9T5Z3kpAKfJTSy6rMT2AAnH3ag1)

### 3 Create File to test secure copy
 #### 3.1 create file
````shell 
echo 'testing connection'> file
````