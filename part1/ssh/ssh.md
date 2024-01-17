**1. Enable SSH authentication through OpenLDAP.**
- install and configure needed packages:
````shell
sudo apt-get install -y openssh-server libpam-ldap nscd
sudo dpkg-reconfigure libpam-ldap
````
- enable **Pluggable Authentication Modules** (PAM)
````shell
sudo sed -i '/^#UsePAM yes/c\UsePAM yes' /etc/ssh/sshd_config
````
---
***What is PAM?***

a framework on Unix-like systems that separates and standardizes authentication services. It allows flexible integration of various authentication methods.

![pam](https://www.researchgate.net/profile/Marcel-Waldvogel/publication/271214682/figure/fig1/AS:392061795749895@1470486224522/Flexibility-of-Pluggable-Authentication-Modules-PAM.png)

---


**2. Restrict SSH access to users belonging to the HR group in OpenLDAP.**
````shell
echo "AllowGroups HR" | sudo tee -a /etc/ssh/sshd_config
sudo systemctl restart ssh
````

**3. Test SSH access for an authorized user and an unauthorized user.**

use command: 
````shell
ssh <username>@<ldap client ip address>
````
