
**1. Configure an OpenLDAP server with a minimum of two users and two groups.**


 a. Installation:

Install the server and the main command line utilities with the following

````shell
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install slapd ldap-utils
````
![slapd config](https://drive.google.com/uc?id=1i_oh_9tQx_Mw680AofmINS2bZmrlKmml)

Install ldap-account-manager and all needed packages to use the GUI of OpenLDAP
````shell
sudo apt install apache2 php php-cgi libapache2-mod-php php-mbstring php-common php-pear -y
sudo apt -y install ldap-account-manager
sudo a2enconf php*-cgi
````

Start slapd.service, apache2 make sure that they're active and runnning
````shell
sudo systemctl start slapd.service
sudo systemctl enable apache2
sudo systemctl start apache2
````
b. Configure server's Hostname:
````shell
sudo hostnamectl set-hostname kali.projet.local
````
![hostname](https://drive.google.com/uc?id=1vrcgg40yFcNHSnrlBa1VilPgRhG8k-pI)

c. Configure Firewall:
````shell
sudo apt-get install ufw
sudo ufw enable
sudo systemctl start ufw
sudo ufw allow ldap
````
d. Check LDAP server is up and running

using command:
````shell
sudo slapcat
````
![slapcat](https://drive.google.com/uc?id=1oQwFbQeASUiwq3K_GskESUchOGhY1Gqz)

e. Configure LDAP Domain Name

cretate [basedn.ldif](./basedn.ldif)  file which specifies organizational units "Departement" and "Groupe" within the "projet.local" domain, including their respective object classes and attributes.
added entries using 'ldapadd'
````shell
ldapadd -x -D cn=admin,dc=projet,dc=local -W -f basedn.ldif
````
![slapcat](https://drive.google.com/uc?id=1oQwFbQeASUiwq3K_GskESUchOGhY1Gqz)

f. Add users and groups

created a hashed password for each one of the users using:
````shell
sudo slappasswd
````
![slappasswd](https://drive.google.com/uc?id=1_GvSfvoe6x5S9OCkZ2YLNS0zqBP4rR8c)

created [ldapusers.ldif](./ldapusers.ldif) file which specifies three user entries with specific attributes, including organizational unit, object classes, user details, and passwords within the "projet.local" domain.
````shell
ldapadd -x -D cn=admin,dc=projet,dc=local -W -f ldapusers.ldif
````
````shell
ldapadd -x -D cn=admin,dc=projet,dc=local -W -f ldapgroups.ldif
````
![users](https://drive.google.com/uc?id=1CeuOdcXyJHh6BnZw56TpVG25apppQ59V)

![groups](https://drive.google.com/uc?id=12hyESzTwXZKWD0SMrO09nIhGEF3wpgwr)

g. Configure ldap-account-manager and check users and groups created
![lam](https://drive.google.com/uc?id=1UF-UZF2jZ4iJkWlNW8AELojnxGEzfaEy)

![treesuffix](https://drive.google.com/uc?id=1VYA3PDEEUZ9s33_Mg4SevzeKyRBCR1xZ)

![serversettings](https://drive.google.com/uc?id=1YbCbtNKWsFkQ2tSSMpDuXNANNyGtB4kn)

![userslam](https://drive.google.com/uc?id=1zOrbIAwlJHbogU984l0Ubxw0wE6QBNaM)

![groupslam](https://drive.google.com/uc?id=1hFwj6wVYzYCwNT5qNXzyGTeIjrsSO72D)

LDAP directory:

| user   | pwd    | 
|--------|--------|
| farah  | farah  |
| ranim  | ranim  |
| chiraz | chiraz |


![tree](https://drive.google.com/uc?id=11EE5EeyW0jBk-ucSlsltIL1fH8uizRpi)


**2. Add custom information, including x509 certificates, for all users.**

- create users certificates for each user in their client machines
````shell
openssl genrsa -out farah-key.priv 2048
openssl rsa -in farah-key.priv -pubout -out farah-key.pub
openssl req -x509 -new -days 3650 farah-key.priv -out farah.cert
````
- send certificate and public key from user's machine to the ldap server's machine
````shell
python3 -m http.server 8000
````
````shell
wget farah@192.168.1.12:8000/farah.cert
wget farah@192.168.1.12:8000/farah-key.pub

# do the same for ranim's and chiraz's files
````
- upload certificate for each user using LAM
![certupload](https://drive.google.com/uc?id=1NZL-R_PthNUW3fIwNLJn1acnr0nqyZqM)
 
**3. Ensure successful user authentication on the OpenLDAP server.**

|      | server            | client             |
|------|-------------------|--------------------|
| FQDN | kali.projet.local | farah.projet.local |
| IP   | 192.168.1.13      | 192.168.1.12       |

- Add server's IP address in the clients /etc/hosts and check connexion using ping
- Install needed packages and configure client's machine
````shell
sudo apt install libnss-ldap libpam-ldap ldap-utils nscd -y
````
- Edit [/etc/nsswitch.conf](nsswitch.conf) file (change passwd, group and shadow attributes to 'ldap compat')
- Edit /etc/pam.d/common-password and  delete "use_authtok"
- Edit /etc/pam.d/common-session and add this line
```shell
session optional pam_mkhomedir.so skel=/etc/skel umask=077
```
- Test user authentication from client's machine

@@@@@@@@   add image here @@@@@@

**4. Test the secure aspect of LDAP with LDAPS and describe its various advantages.**
- generate a self signed server certificate, add it to /etc/ldap/sasl2 and change to appropriate permissions for OpenLDAP 
````shell
openssl genrsa -aes128 -out kali.projet.local.key 4096
openssl rsa -in kali.projet.local.key -out kali.projet.local.key
openssl req -new -days 3650 -key kali.projet.local.key -out kali.projet.local.csr
openssl x509 -in kali.projet.local.csr -out kali.projet.local.crt -req -signkey kali.projet.local.key  -day 3650
sudo chown -R openldap:openldap /etc/ldap/sasl2
````
- create [SSL-LDAP.ldif](SSL-LDAP.ldif) file to add the CA certificate file path, replaces the TLS certificate file and key file paths with updated values for enhanced security
- apply changes using:
````shell
sudo ldapmodify -Y EXTERNAL -H ldapi:/// -f SSL-LDAP.ldif
````
- edit slapd.conf file by adding "ldaps:///" to "SLAPD_Services"
- edit ldap.conf file by adding:
````shell
TLS_CACERT /etc/ldap/sasl2/ca-certificates.crt
TLS_REQCERT allow
````
##### On the client's side:
- check server certificate:
````shell
openssl s_client -connect 192.168.1.13 -showcerts > ldap_server_certs.pem
````
- add server certificate to /etc/ldap/sasl2 and /usr/local/share/ca-certificates
- configure ldap.conf file just like the server
- restart the nscd and test:

![ldaps](https://drive.google.com/uc?id=1-ts3dGjOKdfhQySYb0qzB0z-jP4vvTJM)