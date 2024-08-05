WordPress Installation with Nginx, MariaDB, and PHP
This repository contains Ansible playbooks to set up a WordPress site on an Ubuntu 20.04 server using Nginx, MariaDB, and PHP.

Requirements
Ansible installed on your control machine
Ubuntu 20.04 server(s) with SSH access
Open ports: 80 (HTTP), 443 (HTTPS) for web traffic
Basic knowledge of Ansible and server configuration
Preparation
Clone the repository:

sh
Copy code
git clone https://github.com/SergeyZaika/install-on-ubuntu20.04_wordpress_with_nginx_mariadb_php.git
cd your-repo
Update the hosts file with your server(s) information:

ini
Copy code
[web_sites]
your_server_ip_or_hostname
Variables
domain: Your domain name or server IP address.
php_version: Version of PHP to be installed.
db_name: Name of the WordPress database.
db_user: Database user for WordPress.
db_password: Password for the database user.
root_db_password: Root password for MariaDB.
wordpress_admin_user: Admin username for WordPress.
wordpress_admin_password: Admin password for WordPress.
wordpress_admin_email: Admin email for WordPress.
Installation
Update the install_wordpress.yaml playbook with your domain or IP address:

yaml
Copy code
vars:
  domain: your.domain.com  # If you don't have a domain, use your server's IP address
Run the installation playbook:

sh
Copy code
ansible-playbook -i hosts install_wordpress.yaml
Reinstallation
If you need to reinstall WordPress, run the cleanup playbook first to ensure the environment is clean, and then run the installation playbook again.

Run the cleanup playbook:

sh
Copy code
ansible-playbook -i hosts cleanup_host.yaml
Run the installation playbook:

sh
Copy code
ansible-playbook -i hosts install_wordpress.yaml
Files in the Repository
cleanup_host.yaml - Playbook to clean up the host for a fresh installation
install_wordpress.yaml - Playbook to install and configure WordPress
templates/fastcgi-php.conf.j2 - Template for fastcgi-php.conf configuration
templates/fastcgi_params.j2 - Template for fastcgi_params configuration
templates/nginx.conf.j2 - Template for basic Nginx configuration
templates/nginx_wordpress.j2 - Template for Nginx configuration specific to WordPress
templates/wp-config.php.j2 - Template for WordPress wp-config.php configuration
