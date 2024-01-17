# Secure Authentication Project
### Project Objective
This project's primary aim is to build a secure authentication and network services infrastructure using recognized technologies like OpenLDAP, SSH, Apache, OpenVPN, DNS, and Kerberos.

### Technologies Used:  
<div style="display: flex; justify-content: right; align-items: center;margin-right:50px;">
    <img src="https://www.axonius.com/hubfs/Adapter%20Logos/OpenLDAP%20Logo.png" height="40" width="40" style="margin-right: 10px;" />
    <img src="https://cdn-icons-png.flaticon.com/512/5136/5136897.png" height="40" width="40" style="margin-right: 10px;" />
    <img src="https://britewire.com/wp-content/uploads/apache-http-server-300x300.jpg" height="40" width="40" style="margin-right: 10px;" />
    <img src="https://usermanual.vtenext.com/uploads/images/gallery/2023-03/openvpn-logo.jpg" height="40" width="40" style="margin-right: 10px;" />
    <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTSIsU-44la2KmoDhrT7gUCh2r2dsPVRx_BaQ&usqp=CAU" height="40" width="40" style="margin-right: 10px;" />
    <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQUgsK-4bNt8VqwyLZTlJvD6dkQkyP67ElV7Dg8KW0DrjDfX3ZpDGouBMPakWXXUWW0KCg&usqp=CAU" height="40" width="40" />
</div>



- **OpenLDAP**:  
  (LDAP: Lightweight Directory Access Protocol) serves as the centralized and organized directory service for efficient storage, management, and retrieval of user authentication / authorization data.
  
- **SSH** (Secure Shell):  
    employed for secure remote access to systems, ensuring encrypted communication between the client and the server during authentication and data transfer.
  
- **Apache**:  
  serves as the web server, managing role-based access to web-based applications and providing a secure environment for user interactions.
   
- **OpenVPN**:  
  employed to create a secure Virtual Private Network (VPN) for secure remote access to internal resources through VPN tunnels, ensuring encrypted communication over untrusted networks.
  
- **DNS** (Domain Name System):  
  utilized for domain resolution, mapping domain names to IP addresses, and facilitating seamless access to resources within the network.
  
- **Kerberos**:  
  implemented for secure ticket-based authentication within the network, providing a trusted third-party authentication service.
 ___

## Overview

[](https://drive.google.com/uc?id=)
### [Part 1: Authentication with OpenLDAP, SSH, Apache and OpenVPN](./part1/part1.md)  

Setting up a centralized authentication with OpenLDAP, integrating it with SSH, Apache, and OpenVPN.
Thorough testing ensures secure and controlled access. SSH secures remote system access, Apache acts as a web server with restricted access, and OpenVPN provides a secure VPN solution authenticated through OpenLDAP.

### [Part 2: Management of network services with DNS](./part2/dns.md)  

Establish a Bind DNS server on a dedicated machine, incorporating DNS records for OpenLDAP, Apache, and OpenVPN servers.
Validate the setup by testing DNS resolution for each service, ensuring precise domain name configurations.

### [Part 3: Kerberos Authentication](./part3/kerberos.md)  
Setting up a secure Kerberos server involves installing and configuring the software, defining realms, adding user principals and  their passwords' policies.
Opting for SSH as the authentication service enhances security, particularly for remote access. 


