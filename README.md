#
Imagine you are managing a cluster of Splunk Search Heads. Your goal is to inspect all incoming web requests and check for login attempts by the admin user. If the request comes from the predefined IP address and the username is admin, access is granted. Otherwise, admin login is denied—no one can log in as admin from any other IP.
However, all other users (non-admin accounts) can log in from anywhere, as long as they have access to the Splunk Search Head web interface.
To achieve this, you'll use a load balancer to distribute incoming traffic across your Search Heads. In summary, your virtual machine running OpenResty and HAProxy will handle both access control and request load balancing.
Additionally, this setup hides the IP addresses of your actual Splunk Search Heads—clients will only interact with the load balancer, which forwards their requests appropriately.
#



🔐 OpenResty – Web-Level Access Control
OpenResty is used to enforce web-based access restrictions. For example, you can configure it to allow only specific users—such as an admin—from a specific IP address to access the Splunk Search Head.

Note: This type of restriction is handled at the web request level, not at the network level like a firewall.

⚖️ HAProxy – Load Balancing
HAProxy is used to distribute incoming traffic across multiple Splunk Search Heads using various load-balancing algorithms. This ensures high availability and better performance under load.


This project is tested on Oracle Linux 9.x.x.
#
This project is created by 'Arashk TaqiNiarami' and 'Zahra Sheykhpour'.
All rights reserved.

source: https://openresty.org/en/installation.html
