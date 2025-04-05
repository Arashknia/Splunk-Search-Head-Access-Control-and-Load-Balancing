# This is the scenario:

+ Clients enter: "https://\<IP-Address-of-OpenResty-Machine\>"

+ First, They will be connected to OpenResty, and it checks **Admin-Restriction-Policy** on coming requests.

+ If requests were ok with **Admin-Restriction-Policy**, OpenResty passes the request to HAproxy.

+ HAproxy balances the requests between your search heads.

Installation of HA-Proxy
# Prerequisites
Disabling of SELINUX:
```
setenforce 0
```

# Installation
```
yum install haproxy
```

Setting Capabilities:
```
sudo setcap 'cap_net_bind_service,cap_net_raw,cap_net_admin+eip' /usr/sbin/haproxy
```

