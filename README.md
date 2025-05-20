
# Ansible WordPress Deployment on OrbStack Linux Machines

This project demonstrates how to deploy a WordPress site on OrbStack Linux machines using Ansible.

[OrbStack](https://docs.orbstack.dev/) is a fast, light, and simple way to run containers and Linux machines. It's a supercharged alternative to Docker Desktop and WSL, all in one easy-to-use app.


## Prerequisites

*   OrbStack installed and running.
*   Create an OrbStack [Linux machine](https://docs.orbstack.dev/machines/) using command line or GUI. e.g., `orb create ubuntu:jammy`.
*   Ansible installed on your local machine.
*   Python 3.10.8 and above installed on your OrbStack Linux machine.


## Configuration

*   **inventory**: Defines the target server(s).  Update with your OrbStack Linux machine's IP address or hostname.
*   **group\_vars/webservers**: Contains variables for the deployment, such as:
    *   `owner`: User owner for web files (default: `www-data`)
    *   `group`: Group owner for web files (default: `www-data`)
    *   `domain`: Domain name for the WordPress site (e.g., `ubuntu.orb.local`)
    *   `mysql_root_password`: MySQL root password
    *   `mysql_database`: MySQL database name
    *   `mysql_user`: MySQL user
    *   `mysql_password`: MySQL password
    *   `mysql_db`: Database name for WordPress
    *   `wordpress_url`: URL for the WordPress tarball
    *   `webserver_port`: Webserver port (default: 80)
    *   `webserver_port_secure`: Secure webserver port (default: 443)
    *   `tls_dir`: TLS certificate directory (default: `/etc/letsencrypt/live/`)
    *   `cert_file`: Certificate file name
    *   `key_file`: Key file name
    *   `letsencrypt_email`: Email for Let's Encrypt

## Deployment

1.  **Update Inventory**: Modify the [inventory](inventory) file with the correct connection details for your OrbStack Linux machine.

2.  **Run the Playbook**: Execute the `main.yml` playbook:

    ```bash
    ansible-playbook -i inventory main.yml
    ```

    This playbook will:

    *   Install Nginx, PHP, MySQL, and other dependencies.
    *   Configure Nginx virtual host.
    *   Create MySQL database and user.
    *   Download and extract WordPress.
    *   Set up the `wp-config.php` file.
    *   Restart Nginx and MySQL services.

## Post-Deployment

*   Access your WordPress site in your browser using the domain name defined in `group_vars/webservers`.

## SSL/TLS (HTTPS)

The playbook includes commented-out tasks for setting up SSL/TLS certificates using Certbot.  These tasks are disabled because OrbStack Linux machines automatically create SSL certificates. If you are not using OrbStack, you can uncomment these tasks to enable automatic certificate generation.

## Notes

*   The `php_version` variable in [main.yml](main.yml) is set to "8.3". Ensure this version is available on your target system.
*   The MySQL root password is set in the `group_vars/webservers` file.  Change this to a strong, secure password.
*   This setup is designed for Ubuntu/Debian-based systems.  Adjust the package names and service names accordingly for other distributions.

![This is how it looks like ](<icon/wordpress.jpeg>)
