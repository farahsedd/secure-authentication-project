#Authentification avec Kerberos

### Kerberos Overview
Kerberos is a network authentication protocol that enables secure communication by verifying the identities of users and services using symmetric key cryptography.  
It relies on a trusted Key Distribution Center (KDC) to issue tickets for authentication, preventing unauthorized access and eavesdropping in networked environments.

In the authentication process, a user requests a service, the authentication server verifies the user's identity and issues a Ticket-Granting Ticket (TGT).  
The user then obtains a service ticket from the TGS using the TGT and finally gains access to the desired service by presenting the service ticket.

### Server Configuration 

#### step 1 : configure hostname
```shell
sudo nano /etc/hosts
hostnamectl --static set-hostname kdc.sever.tn  
````

#### step 2 : Edit Kerberos Configuration File 
Edit the Kerberos configuration file (/etc/krb5.conf) to define the realm, KDC information, and other settings.

#### Step 3: Install kerberos
```shell
sudo apt-get update
sudo apt install krb5-kdc krb5-admin-server krb5-config
````

#### step 4:  initialize a new Kerberos realm and master key/password
```shell
sudo su
cd /etc/krb5kdc
krb5_newrealm
````
[kerberos config ](https://drive.google.com/uc?id=1759EyWSv7EmZ1t1hBv2SgcgJEFBzl26F)
[ ](https://drive.google.com/uc?uc?export=download&id=1aEg_RhGOO_j-ACWHygjyFBGHWSG925KT)

#### step 5 : add princiapal users   
Use the kadmin.local utility to create principals for the KDC admin
```shell
kadmin.local:add_principal utilisateur
kadmin.local:get_principal utilisateur
kadmin.local:get principals 
````
[add principals ](https://drive.google.com/uc?id=1VmcCmY0tvekGIu_aXFkEkTS25mJZWDSQ)
[ ](https://drive.google.com/uc?id=1ISKxCIQA7wF-ggEM0NwlhKxJ8AeV4Ow8)


#### Step 6:  Create a Keytab File
````shell  
file kadm5.keytab
````
[](https://drive.google.com/uc?id=1pOWQW8IlNPCeckNyDTyVeRU8q9qCBvJs)

#### step 7 : create  ticket and add it to keytab file
````shell  
kinit 
klist -kte kadm5.keytab
````

[ticket generating](https://drive.google.com/uc?id=1eF-4tgYkZxQWudmYfHKKV72MShNUgGWr)
[](https://drive.google.com/uc?id=1pOWQW8IlNPCeckNyDTyVeRU8q9qCBvJs)

#### step 8 : add policies to each user
````shell  
  addent -password -p root/kdc@SERVER.TN -k 1 -e aes256-cts-hmac-sha1-96
  wkt kadm5.keytab
  rkt kadm5.keytab
  l

````
[add policies](https://drive.google.com/uc?id=187BzGtxu9_aywQ7qiXh9NE9mM9oNB30z)
[](https://drive.google.com/uc?id=1sQMvIOX0l6Zy38xrbHnL5M11jAj4_IEW)
[](https://drive.google.com/uc?id=1eF-4tgYkZxQWudmYfHKKV72MShNUgGWr)

#### Step 9: install ssh ,edit sshd_config and ssh_config file
````shell  
apt install openssh-server
````
[install ssh](https://drive.google.com/uc?id=1eF-4tgYkZxQWudmYfHKKV72MShNUgGWr)

Uncomment and set GSSAPIAuthentication and GSSAPICleanUpCreadentials to yes in SSH configuration files. Restart the SSH service.  

[configure ssh files](https://drive.google.com/uc?id=16PHX6iz28uoeYwWy-D_kdbjJnOdRIE3E)

#### Step 10:Create a Service Principal for SSH
````shell  
sudo kadmin.local
addprinc -randkey host/kdc.SERVER.tn
ktadd -k host/kdc.SERVER.tn
````
[create ssh service](https://drive.google.com/uc?id=16LVP-B5Obh1X1dHMKbOj1BYFeswuCq6X)

#### Step 11 : create a new user account and log using it
````shell  
adduser utilisateur
su -l utilisateur 
````
[create new user](https://drive.google.com/uc?id=17QAirmxXo8_Gx4-ZvHGu8BbjBIUtO1JL)

#### Step 12 :  authenticate using TGT 
get a ticket to be able to authenticate without writing mdp
````shell 
kinit
klist
ssh kdc.server.tn
w 
````
[authenticating using TGT](https://drive.google.com/uc?id=1GuWaL9T5Z3kpAKfJTSy6rMT2AAnH3ag1)
