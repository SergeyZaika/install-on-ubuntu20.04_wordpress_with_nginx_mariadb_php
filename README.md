WordPress Installation with Nginx, MariaDB, and PHP on Ubuntu 20.04
This repository contains Ansible playbooks and templates to set up one or more WordPress sites on an Ubuntu 20.04 server using Nginx, MariaDB, and PHP. Additionally, it includes a playbook for cleaning up the environment if you need to reinstall.

Requirements
Ansible installed on your control machine
Ubuntu 20.04 server(s) with SSH access
Open ports: 80 (HTTP), 443 (HTTPS) for web traffic
Basic knowledge of Ansible and server configuration
Variables
General Variables
domain: The domain name or IP address for your site.
second_domain: The domain name or IP address for your second site.
ubuntu_user: The username for your Ubuntu server (default: ubuntu).
php_version: Version of PHP to be installed (e.g., "8.2").
db_name: The name of the WordPress database.
db_user: The username for the WordPress database.
db_password: The password for the WordPress database user.
root_db_password: The root password for MariaDB.
wordpress_admin_user: The admin username for WordPress.
wordpress_admin_password: The admin password for WordPress.
wordpress_admin_email: The admin email for WordPress.
remote_wordpress_file: Path to the downloaded WordPress archive. Note: You need to prepare the WordPress archive yourself and download it to the specified path. The file should be in .tar.gz format.
wordpress_install_dir: Installation directory for the WordPress site.
second_wordpress_install_dir: Installation directory for the second WordPress site.
Secret Variables
The sensitive data (e.g., passwords) are stored in a separate vars_secret.yaml file, which is encrypted using Ansible Vault.

Creating the vars_secret.yaml File
You need to create the vars_secret.yaml file with the following structure:

yaml
Copy code
# vars_secret.yaml
db_password: your_db_password
root_db_password: your_root_db_password
wordpress_admin_password: your_wordpress_admin_password
To encrypt the vars_secret.yaml file with Ansible Vault, run:

bash
Copy code
ansible-vault encrypt vars_secret.yaml
When running the playbooks, you'll need to provide the vault password using the --ask-vault-pass option.

Usage
Install the First WordPress Site
To install the first WordPress site:

bash
Copy code
ansible-playbook -i hosts install_wordpress.yaml --ask-vault-pass
This playbook sets up the first WordPress site at the specified domain. It installs Nginx, MariaDB, PHP, and configures the server.

Obtain SSL Certificates
To secure your site with SSL, run:

bash
Copy code
ansible-playbook -i hosts certbot.yaml --ask-vault-pass
This playbook uses Certbot to obtain and install an SSL certificate for your WordPress site.

Install the Second WordPress Site
To install a second WordPress site on the same server:

bash
Copy code
ansible-playbook -i hosts install_second_wordpress.yaml --ask-vault-pass
This playbook sets up a second WordPress site with its own database and user credentials, without interfering with the first site.

Clean Up the Host for Reinstallation
If you need to clean up the server for a fresh installation:

bash
Copy code
ansible-playbook -i hosts cleanup_host.yaml --ask-vault-pass
This playbook stops and removes all installed services (Nginx, MariaDB, PHP), and deletes configuration and data files, returning the server to a clean state.

Templates Explanation
fastcgi-php.conf.j2: Configuration snippet for handling PHP requests in Nginx.
fastcgi_params.j2: Defines parameters for FastCGI processes.
mime.types.j2: Provides MIME type mappings for Nginx.
nginx.conf.j2: Basic configuration file for Nginx.
nginx_wordpress.j2: Nginx configuration for serving the first WordPress site (HTTP only).
nginx_wordpress_ssl.j2: Nginx configuration for serving the first WordPress site with SSL.
nginx_wordpress_ssl_second.j2: Nginx configuration for serving the second WordPress site with SSL.
options-ssl-nginx.conf.j2: SSL configuration options for Nginx, used by Certbot.
wp-config.php.j2: The main configuration file for WordPress, customized for the first site.
wp-config_second.j2: Configuration file for the second WordPress site.
Multiple Sites
You can use these playbooks to manage multiple WordPress sites on the same server. Each site will have its own database, user credentials, and SSL certificate. The provided templates ensure that configurations for each site do not conflict.

Example Workflow
Install the first WordPress site:

bash
Copy code
ansible-playbook -i hosts install_wordpress.yaml --ask-vault-pass
Obtain an SSL certificate:

bash
Copy code
ansible-playbook -i hosts certbot.yaml --ask-vault-pass
Install the second WordPress site:

bash
Copy code
ansible-playbook -i hosts install_second_wordpress.yaml --ask-vault-pass
If needed, clean up the server:

bash
Copy code
ansible-playbook -i hosts cleanup_host.yaml --ask-vault-pass