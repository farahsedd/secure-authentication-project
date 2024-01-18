# Authentication with Kerberos

## Kerberos Overview  
Kerberos is a secure network authentication protocol that verifies user and service identities using symmetric key cryptography. It relies on a trusted Key Distribution Center (KDC) to issue authentication tickets, preventing unauthorized access. During authentication, a user requests a service, and the KDC issues a Ticket-Granting Ticket (TGT).

### [Section 1 :Kerberos Server and OpenSSH](./server.md)
The Kerberos Server manages authentication and ticket issuance. OpenSSH is configured to use Kerberos, integrating server settings like realms and principals. Users need valid Kerberos tickets to authenticate via SSH, enhancing security.

### [Section 2 :Kerberos Client and SSH](./client.md) 
Configuring a machine as a Kerberos client involves specifying client settings and integrating SSH authentication with Kerberos tickets. Users authenticate to SSH using Kerberos tickets, bolstering the security of connections.



[Go Back to Previous Section](../README.md)