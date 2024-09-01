# (Linux) [Easy] Challenge 012A0094
## Author: David Chaparro - davidch09
## Points: User flag: 10 - System flag: 20

#### Requirements (Not an expert, only know the concept)

##### Topics
+ Linux
+ Port/Web enumeration

##### Tools
+ nmap
+ gobuster

##### Languages / Ciphering
+ 
+ 


## Solution

### Ports Enumeration

We start with my prefer port enumeration, with the `nmap` command:

```sh
sudo nmap -p- -sS -Pn --min-rate 5000 -vvv -n -oG allPorts.txt 10.129.231.219
```

We discover these open ports:

```java
PORT      STATE SERVICE      REASON
22/tcp    open  ssh          syn-ack ttl 63
53/tcp    open  domain       syn-ack ttl 63
80/tcp    open  http         syn-ack ttl 63
1557/tcp  open  arbortext-lm syn-ack ttl 63
32400/tcp open  plex         syn-ack ttl 63
32469/tcp open  unknown      syn-ack ttl 63
```
We see that a web service is running in the port `80` and a service named `plex` running in the port `32400`. Now let's analyze deeper the services and versions with this command:

```sh
sudo nmap -p 22,53,80,1557,32400,32469 -Pn -sVC --min-rate 5000 -oN puertos.txt 10.129.231.219
```

```java
PORT      STATE SERVICE VERSION
22/tcp    open  ssh     OpenSSH 6.7p1 Debian 5+deb8u3 (protocol 2.0)
| ssh-hostkey: 
|   1024 aa:ef:5c:e0:8e:86:97:82:47:ff:4a:e5:40:18:90:c5 (DSA)
|   2048 e8:c1:9d:c5:43:ab:fe:61:23:3b:d7:e4:af:9b:74:18 (RSA)
|   256 b6:a0:78:38:d0:c8:10:94:8b:44:b2:ea:a0:17:42:2b (ECDSA)
|_  256 4d:68:40:f7:20:c4:e5:52:80:7a:44:38:b8:a2:a7:52 (ED25519)
53/tcp    open  domain  dnsmasq 2.76
| dns-nsid: 
|_  bind.version: dnsmasq-2.76
80/tcp    open  http    lighttpd 1.4.35
|_http-title: Site doesnt have a title (text/html; charset=UTF-8).
|_http-server-header: lighttpd/1.4.35
1557/tcp  open  upnp    Platinum UPnP 1.0.5.13 (UPnP/1.0 DLNADOC/1.50)
32400/tcp open  http    Plex Media Server httpd
|_http-favicon: Plex
|_http-cors: HEAD GET POST PUT DELETE OPTIONS
| http-auth: 
| HTTP/1.1 401 Unauthorized\x0D
|_  Server returned status 401 but no WWW-Authenticate header.
|_http-title: Unauthorized
32469/tcp open  upnp    Platinum UPnP 1.0.5.13 (UPnP/1.0 DLNADOC/1.50)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel
```

### Web enumeration

Okay, after exploring the web services in both ports, we see that the one in the port `80` doesn't have anything (the main page is empty, but we can do directory/subdomain enumeration later) and the `Plex` service doesn't appear to be vulnerable, because after sign in and see the version of the service, we can check it out using `searchsploit`:  



![1](./assets/1.png)

#### parameters explanation

+ -p- : Scans all possible ports (1 - 65535)
+ -sS : (stealth Scan) Sends 

jeez, that looks like an encoded message, but the last `=` character can tell us is a **base 64** cipher, we are gonna use https://cyberchef.io/ to analyze it:

![5](./assets/5.png)

This looks like **binary** code, after decoding it from binary, we get our precious flag:

![6](./assets/6.png)



