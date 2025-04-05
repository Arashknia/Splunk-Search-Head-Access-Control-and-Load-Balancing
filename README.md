🔐 OpenResty – Web-Level Access Control
OpenResty is used to enforce web-based access restrictions. For example, you can configure it to allow only specific users—such as an admin—from a specific IP address to access the Splunk Search Head.

Note: This type of restriction is handled at the web request level, not at the network level like a firewall.

⚖️ HAProxy – Load Balancing
HAProxy is used to distribute incoming traffic across multiple Splunk Search Heads using various load-balancing algorithms. This ensures high availability and better performance under load.

This project is tested on Oracle Linux 9.x.x.
This project is created by Arashk Taqi Niarami and Zahra Sheykhpour.
All rights reserved.

source: https://openresty.org/en/installation.html
