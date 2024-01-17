# DNS ,Domain Name System 

DNS is a system that translates user-friendly domain names (like www.example.com) into IP addresses that computers use to locate each other on the internet.   

It serves as a directory for the internet, making it easier for people to access websites without needing to remember numerical IP addresses.   

A DNS server, or Domain Name System server, is a critical component of the internet infrastructure that translates human-readable domain names into machine-readable IP addresses.  
 ## Configuration of a DNS server
1.1 Configurez un serveur DNS (Bind) sur une machine distincte.  

1.1.1 Install BIND DNS Server && dnsutils package
```shell
sudo apt-get update
sudo apt-get install bind9
sudo apt install dnsutils
````
1.1.2  Adjust BIND Configuration Files: 

Edit the BIND configuration file, typically located at /etc/bind/named.conf.options, and customize settings:

![](https://drive.google.com/uc?1DYRdM9p3NgVWjqXC6zxWW9LPAXMODZ9g)
![](https://drive.google.com/uc?id=10uNp7FZGICTJ3DxqadtgxgQP0XR4LJNB)

![](https://drive.google.com/uc?id=1cqBq-mZbhI1xGbc8KJLtJeOqhNf2fyxD)
![](https://drive.google.com/uc?id=1asdTi2b0v346VWj3ZyvV3Bv_RMQ320E-)

1.1.3  Specify Domain Name:  

Edit the main BIND configuration file and specify your domain name:

![](https://drive.google.com/uc?id=1Xiqt3XSshVMWRDEI-8e3A43qKVtme6Rg)
![](https://drive.google.com/uc?id=1ui85qwSzHByN258utclM4EJoZhS__Lcq)

1.1.4  Create DNS Zone Files  
Creating DNS zone files involves defining resource records (RRs) for a domain, specifying information like IP addresses, mail servers, and other associated resources

![](https://drive.google.com/uc?id=1tGGoO1O_SjmZOfMVyNeqfUdFODfCrFwy)
![](https://drive.google.com/uc?id=1zZgTcrxOJDGgibmhEhX7qgx8SOxYCfj4)


1.1.5  Create Reverse DNS Zone  
A reverse zone file, often called a Reverse DNS (rDNS) maps IP addresses to hostnames, aiding in identification, verification, and overall network management

![](https://drive.google.com/uc?id=1cGZ4k75cW7YPlBLkc76y9qXYEv69rb75)
![](https://drive.google.com/uc?id=1MyDOw3FwgyWq0OtN8O6O919Y-PDzyAgr)
![](https://drive.google.com/uc?id=10yD0V4tXUNXcRnIkwAj4Hd5hiSS-gOyh)

1.1.6  Edit resolv.conf file : setting search domains, and defining resolver option.

![](https://drive.google.com/uc?id=1Ftql3Gb3vPsEW1NEpQc-WfSe4ohy6HAs)
![](https://drive.google.com/uc?id=1aEWvBbkgttLGZmvRRF4Z7sNqpsHLUptk)

1.1.7  Check Configuration Files and Dns Zone file:  
ensures the correct syntax and structure, helping to prevent errors and ensure the proper functioning of the DNS zone within the BIND DNS server.

![](https://drive.google.com/uc?id=1-rnGxg5owPdd9cHL9j3H7UdBy_wX8PYG)
![](https://drive.google.com/uc?id=1WdhHuCC24mtX9_vWCrQLzcih_K21XI91)

1.1.8 Restart server and verify resolution
![](https://drive.google.com/uc?id=1qdw5OymNAJD3tsPx-qMaPrmcOMGQmFT7)
![](https://drive.google.com/uc?id=12_JVrdK-3UjRgiSuUhho4q3qeJHxZ447)
![](https://drive.google.com/uc?id=152z_cfKNTvyFq1BN9xxYsxVRYT-HO4av)