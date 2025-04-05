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

# Configuration

**Step 1: splunk_admin_restriction.conf configuration

Create the following directory:

```
sudo mkdir -p /usr/local/openresty/nginx/conf/conf.d/
```

🧐 Create the config file "splunk_admin_restriction.conf" by running the following command and add **splunk_admin_restriction** file content to this file.
```
vi /usr/local/openresty/nginx/conf/conf.d/splunk_admin_restriction.conf
```

```
# OpenResty NGINX + Lua after installing HAProxy
# ===============================================
# This config file is to satisfy both requirements

# Redirect HTTP to HTTPS
server {
    listen 80;
    server_name 192.168.101.110;
    return 301 https://$host$request_uri;
}

# HTTPS with Lua IP-based restriction for admin login
server {
    listen 443 ssl;
    server_name 192.168.101.110;

    ssl_certificate /usr/local/openresty/nginx/ssl/nginx-selfsigned.crt;
    ssl_certificate_key /usr/local/openresty/nginx/ssl/nginx-selfsigned.key;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_prefer_server_ciphers on;
    ssl_ciphers HIGH:!aNULL:!MD5;

    # Admin login check via Lua
    location ~ ^/[^/]+/account/login$ {
        access_by_lua_block {
            local ngx = ngx
            local cjson = require "cjson"

            if ngx.req.get_method() == "POST" then
                ngx.req.read_body()
                local args = ngx.req.get_post_args()

                local username = args["username"]
                local client_ip = ngx.var.remote_addr

                ngx.log(ngx.ERR, "Login attempt from IP: " .. client_ip .. ", username: " .. (username or "nil"))

                if username == "admin" and client_ip ~= "192.168.100.127" then
                    ngx.status = ngx.HTTP_FORBIDDEN
                    ngx.say("403 Forbidden: Admin login not allowed from this IP")
                    return ngx.exit(ngx.HTTP_FORBIDDEN)
                end
            end
        }

        # Proxy to HAProxy
        proxy_pass http://127.0.0.1:9000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    # All other requests to HAProxy
    location / {
        proxy_pass http://127.0.0.1:9000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }
}
```
