# This is the scenario:

+ Clients enter: "https://\<IP-Address-of-OpenResty-Machine\>"

+ First, They will be connected to OpenResty, and it checks **Admin-Restriction-Policy** on coming requests.

+ If requests were ok with **Admin-Restriction-Policy**, OpenResty passes the request to HAproxy.

+ HAproxy balances the requests between your search heads.

# Install OpenResty

**Step 1: Prerequisite**

Before installing OpenResty, make sure that your system is up-to-date and has the required dependencies.
```
sudo yum update
sudo yum install -y gcc pcre-devel zlib-devel make unzip openssl-devel perl
```

**Step 2: Preparing tar package**

Now you should put the **openresty-1.27.1.1.tar.gz** in the root directory.

Run the following commands as root in the right order. 🔥 **Do not change the order of commands.**

```
tar -zxvf openresty-1.27.1.1.tar.gz
```

```
cd openresty-1.27.1.1
```

```
./configure -j2
```

```
make -j2
```

```
sudo make install
```

```
cd ~
```
