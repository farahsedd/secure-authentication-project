### A ***VPN*** (Virtual Private Network) 
is a secure, encrypted connection that enables users to access a private network over the internet. It ensures privacy and data security by creating a protected tunnel between the user's device and the network server, commonly used for secure remote access and private internet browsing.

![vpn schema](https://fortinetweb.s3.amazonaws.com/docs.fortinet.com/v2/resources/7eefb386-9916-11e9-81a4-00505692583a/images/e4bba407cc0825073e57c9d0aea7b1c4_diagram-ldap.png)
---
**4.1 Install and configure OpenVPN for OpenLDAP authentication.**

a. Set Up and Configure an OpenVPN Server
- Install needed packages
````shell
sudo apt update
sudo apt install openvpn easy-rsa
````
- create a new directory on the OpenVPN Server as non-root user called ~/easy-rsa
````shell
mkdir ~/easy-rsa
````
- create a symlink from the easyrsa script that the package installed into the ~/easy-rsa directory and add user permissions
````shell
ln -s /usr/share/easy-rsa/* ~/easy-rsa/
sudo chown ranim ~/easy-rsa
chmod 700 ~/easy-rsa
````
- Create a PKI for Openvpn

PKI directory on the openvpn server will manage the server and clients’ certificate requests instead of making them directly on your CA server

````shell
cd ~/easy-rsa
nano vars
````
![vars](https://drive.google.com/uc?id=1Dpg9HUgE1VLwqE1DsGtDGnxmj1YHvFk9)

This will ensure that your private keys and certificate requests are configured to use modern Elliptic Curve Cryptography (ECC) to generate keys and secure signatures for your clients and OpenVPN server

This means when a client and server attempt to establish a shared symmetric key, they can use Elliptic Curve algorithms to do their exchange

![init-pki](https://drive.google.com/uc?id=16oxxoiTc2kqvCvU7Df2-IwNHMYffNR8E)

- Create server certificate request and private key

![ger req and key](https://drive.google.com/uc?id=1rSqEOnhGbR5d1aOeYbW81Wm8Vc8Uhufk)

````shell
sudo cp /home/sammy/easy-rsa/pki/private/server.key /etc/openvpn/server/
````

- create CA certificate 
![build-ca](https://drive.google.com/uc?id=1kWJP1vuug5QgdanR5bVI77Mr8XEwvLkG)

- sign server request
![sign-req](https://drive.google.com/uc?id=1bDaxSAM3sHL8Hh4FfkEeYtd_-6eIKvRW)

- create client certificate
![client cert](https://drive.google.com/uc?id=1lT6_w3V-GOb3YIdC-RpYgcDFwZHrbyR1)
![client cert](https://drive.google.com/uc?id=19T4FbskgKU9LzQxmswVWNrQOtxUjU1XL)

- configure /etc/openvpn/server/server.conf
![server.conf](https://drive.google.com/uc?id=1x-hyGuv7cEb4H7hYdkGLFgIHpe29IGj7)
![server.conf](https://drive.google.com/uc?id=1QeOZWZJVxRcVLPkug3lS4No_2QtcFb7G)
![server.conf](https://drive.google.com/uc?id=1RclSuJj7d7rfYKrZbKivavy8ShhD3i3B)
![server.conf](https://drive.google.com/uc?id=15GmWSVNHMtkeQCO9Os6eLR2cn6IfFNMI)
![server.conf](https://drive.google.com/uc?id=1cV18esSazMql0s5_apl6IWQr1AMPu6Of)
![server.conf](https://drive.google.com/uc?id=1sC3DNXDxPO08ycWI1yIYIQOQUAmsnqbf)
![server.conf](https://drive.google.com/uc?id=1ZQvd6qzzO9rBmahXOcfWUuIP9ja5wDng)

- configure /etc/openvpn/auth/ldap.conf
![ldap.conf](https://drive.google.com/uc?id=1HWIKql1t295vP1uRvYFhwKdjIz0aSisr)

- configure /etc/openvpn/client/client.ovpn
![client.ovpn](https://drive.google.com/uc?id=1-5lg0XNWKsfTr11jtH6zutu19GX991n1)

````shell
sudo systemctl restart openvpn-server@server.service
````

**4.2 Test the VPN connection using OpenLDAP credentials.**

![ranim test](https://drive.google.com/uc?id=1sQC_g5cL7n8HsKJ5itejJtpL0tBqeJCp)


**4.3 Test the ability of an authorized client and an unauthorized client to initiate a VPN tunnel.**

![farah test](https://drive.google.com/uc?id=1mCcLSF3v3dQf9puvv3g7L93jb4AS8iBP)

