**3.1 Configure Apache for OpenLDAP authentication. & 3.2 Ensure web page access is restricted to members of the appropriate group in OpenLDAP.****

- enable ldap's needed modules
````shell
sudo a2enmod ldap authnz_ldap
sudo systemctl restart apache2
````
````shell
sudo nano /etc/apache2/sites-available/auth-ldap.conf
````

![auth-ldap.conf](https://drive.google.com/uc?id=1AFXgKWIWJmzRXQHwq6hfx5ohQcxTivlt)

````shell
sudo mkdir /var/www/html/auth-ldap
sudo a2ensite auth-ldap
sudo systemctl restart apache2
sudo nano /var/www/html/auth-ldap/index.html
````

create the [html page](index.html) that a user will see if authenticated

**3.3 Test access for an authorized and an unauthorized user on a selected website.**

***for a user who does not in HR group (does not have access)***

![farah](https://drive.google.com/uc?id=1KJmfLfWfJMGTZ9I7SFcWD6LtnmZbZk-j)
![farah result](https://drive.google.com/uc?id=1Hp2z9LWKUy3FGn2vHjDf1xchylRiAicY)


![ranim](https://drive.google.com/uc?id=1DEwcOSl4CFC6jOdpZw14lYbkalyl1kR0)
![ranim result](https://drive.google.com/uc?id=10B63q5j96V5rcPDu7-dEFKOvPsIBV2PU)