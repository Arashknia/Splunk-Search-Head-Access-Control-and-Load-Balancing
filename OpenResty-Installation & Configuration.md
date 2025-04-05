# Admin restriction policy

This is the scenario:

+ Clients enter: "https://\<IP-Address-of-OpenResty-Machine\>"

+ First, They will be connected to OpenResty, and it checks **Admin-Restriction-Policy** on coming requests.

+ If requests were ok with **Admin-Restriction-Policy**, OpenResty passes the request to HAproxy.

+ HAproxy balances the requests between your search heads.

# Install OpenResty

**Step 1: Install Required Dependencies**

Before installing OpenResty, make sure that your system is up-to-date and has the required dependencies.
```
sudo yum update
sudo yum install -y gcc pcre-devel zlib-devel make unzip
sudo yum install gcc make pcre-devel zlib-devel openssl-devel 
```
