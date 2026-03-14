# Nagios Installation Guide on CentOS

**📊 What is Nagios?** Nagios is a utility designed to monitor network services such as HTTP, SMTP, POP3, ICMP, NNTP, monitor CPU usage, disk space and other hardware. Nagios can also send alerts in case of any problems with the above services or hardware.

**Prerequisite:** To install Nagios, you need a clean server running CentOS.

## Quick Installation

### Step 1: Download and Run the Installation Script

```
curl https://assets.nagios.com/downloads/nagiosxi/install.sh | sh
```

**⚠️ Important:** This script will install Nagios XI (the enterprise version) with all dependencies. The installation process may take 10-15 minutes depending on your server's performance and internet connection.

### Step 2: Wait for Installation to Complete

The script will automatically:

*   Install required dependencies (PHP, Apache, MySQL, etc.)
*   Download and compile Nagios Core
*   Install Nagios XI components
*   Configure Apache virtual host
*   Set up database
*   Start necessary services

## Post-Installation Configuration

### Step 3: Access the Web Interface

After installation completes, open your web browser and navigate to:

```
http://[your-server-ip]/nagiosxi/
```

🔧 Nagios Web Interface - Initial Setup Screen

### Step 4: Initial Configuration

On the first access, you'll need to configure:

*   **Nagios Web Interface URL:** Confirm or change the URL
*   **Time Zone:** Select your local time zone
*   **Program Language:** Choose your preferred language
*   **Theme:** Select the interface theme
*   **License Type:** Accept the license agreement

⚙️ Nagios Configuration - License and Settings

### Step 5: Create Admin Account

You have two options:

*   **Use ready-made credentials:** The installer creates default admin credentials
*   **Create custom credentials:** Set your own username and password

👤 Nagios - Admin Account Creation

### Step 6: Login and Start Monitoring

After creating your account, log in to the Nagios web interface:

📊 Nagios Dashboard - Main Monitoring Interface

📈 Nagios - Service Monitoring View

## Default Credentials (if using automatic setup)

| Component | Username | Password |
| --- | --- | --- |
| Nagios XI Admin | admin | admin |
| MySQL Database | root | nagiosxi |

**⚠️ Security Warning:** Change default passwords immediately after first login!

## Verifying Installation

### Check Nagios Services

```
# Check Nagios Core status
sudo systemctl status nagios

# Check Apache status
sudo systemctl status httpd

# Check MySQL/MariaDB status
sudo systemctl status mariadb

# Check Nagios XI components
sudo /usr/local/nagiosxi/scripts/repair_databases.sh
```

### Test Monitoring

From the Nagios XI dashboard, you can:

*   View host and service status
*   Check network services (HTTP, SMTP, POP3, etc.)
*   Monitor CPU usage and disk space
*   Configure alert notifications
*   Generate reports

## Basic Configuration Examples

### Add a Host to Monitor

In Nagios XI, use the web interface to add hosts:

1.  Go to **Configure → Core Config Manager**
2.  Click on **Hosts → Add New**
3.  Enter host details (name, IP address, etc.)
4.  Apply configuration

### Configure Email Notifications

```
# Edit Nagios contacts
sudo nano /usr/local/nagios/etc/objects/contacts.cfg

# Add or modify email address
define contact {
    contact_name            admin
    use                     generic-contact
    alias                   Nagios Admin
    email                   your-email@example.com
}

# Restart Nagios
sudo systemctl restart nagios
```

## Troubleshooting

| Issue | Solution |
| --- | --- |
| Cannot access web interface | Check if Apache is running: `sudo systemctl status httpd`  <br>Check firewall: `sudo firewall-cmd --list-all` |
| Installation script fails | Check internet connection: `ping -c 4 google.com`  <br>Check disk space: `df -h`  <br>Check memory: `free -m` |
| Nagios not monitoring hosts | Verify configuration: `/usr/local/nagios/bin/nagios -v /usr/local/nagios/etc/nagios.cfg` |
| Email notifications not working | Install mail client: `sudo yum install mailx`  <br>Check mail logs: `tail -f /var/log/maillog` |

## Firewall Configuration

```
# Allow HTTP/HTTPS access to Nagios web interface
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload

# If you need to allow NRPE (Nagios Remote Plugin Executor)
sudo firewall-cmd --permanent --add-port=5666/tcp
sudo firewall-cmd --reload
```

## Useful Commands

| Command | Description |
| --- | --- |
| `sudo systemctl start nagios` | Start Nagios Core |
| `sudo systemctl stop nagios` | Stop Nagios Core |
| `sudo systemctl restart nagios` | Restart Nagios Core |
| `sudo systemctl status nagios` | Check Nagios status |
| `/usr/local/nagiosxi/scripts/reconfigure_nagios.sh` | Reconfigure Nagios XI |
| `/usr/local/nagiosxi/scripts/backup_xi.sh` | Backup Nagios XI configuration |
| `tail -f /usr/local/nagiosxi/var/cmdsubsys.log` | View Nagios XI command logs |

## Uninstalling Nagios

**⚠️ Caution:** This will completely remove Nagios and all its components.

```
# Stop Nagios services
sudo systemctl stop nagios
sudo systemctl stop httpd
sudo systemctl stop mariadb

# Remove Nagios XI (if using the enterprise version)
sudo /usr/local/nagiosxi/scripts/uninstall.sh

# Or remove Nagios Core manually
sudo yum remove nagios nagios-plugins
sudo rm -rf /usr/local/nagios
sudo rm -rf /usr/local/nagiosxi
sudo rm -rf /var/log/nagios
sudo rm -rf /etc/nagios
```

## Next Steps

*   Explore the Nagios XI web interface and familiarize yourself with the dashboard
*   Add servers and services to monitor
*   Configure notification alerts (email, SMS, Slack, etc.)
*   Set up monitoring for specific applications (web servers, databases, etc.)
*   Create custom service checks and plugins
