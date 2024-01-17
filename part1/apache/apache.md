**3.1 Configure Apache for OpenLDAP authentication.**

- enable ldap's needed modules
````shell
sudo a2enmod ldap authnz_ldap
sudo systemctl restart apache2
````
- edit file /etc/apache2/sites-availables/#########

**3.2 Ensure web page access is restricted to members of the appropriate group in OpenLDAP.**


**3.3 Test access for an authorized and an unauthorized user on a selected website.**

![]()
