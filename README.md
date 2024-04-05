## Project Context

The local library in our little town faces budget constraints, prompting consideration of Linux as a cost-effective alternative to Windows. To address user skepticism, our local IT company will demonstrate a Linux setup using virtual machines and an internal virtual network. This demonstration includes a server and a workstation, showcasing their functionalities and capabilities.

### Must-Have Setup

#### Server Configuration

- **Operating System**: Linux (no GUI)
- **Services**:
  - DHCP: isc-dhcp-server (serving a scoped internal network)
  - DNS: bind (resolving internal resources, with a redirector for external resources)
  - HTTP + MariaDB: GLPI (internal website)
- **Backup Strategy**:
  - Weekly backup of configuration files for each service into a single compressed archive.
  - Optional: Backups are placed on a partition located on a separate disk and then unmounted.
- **Remote Management**:
  - SSH for remote management of the server.

#### Workstation Configuration

- **Operating System**: Linux with Desktop Environment
- **Applications**:
  - LibreOffice
  - Gimp
  - Mullvad browser
- **Networking**:
  - Automatic addressing for network configuration
- **File System**:
  - /home folder located on a separate partition on the same disk.
- **Optional Functionality**:
  - Propose and implement a solution to remotely help a user.

### Additional Functionality Proposal

1. **Monitoring and Logging**: Implement monitoring tools like Nagios or Zabbix to monitor server health, service availability, and resource usage. Configure logging to centralize logs for easier troubleshooting.

2. **Security Measures**: Set up firewall rules using iptables or ufw to control traffic to and from the server. Implement fail2ban for automated protection against brute-force attacks.

3. **Automated Deployment**: Use configuration management tools like Ansible or Puppet to automate the deployment and configuration of the server and workstation. This ensures consistency and scalability.

4. **Documentation**: Create detailed documentation outlining the setup process, configuration, and maintenance procedures. This will aid in future deployments and troubleshooting.

5. **User Training**: Provide training sessions for library staff on using Linux and the applications installed on the workstation. This will help alleviate skepticism and ensure a smooth transition.

By implementing these additional functionalities, we can demonstrate not only the basic setup but also the robustness, security, and manageability of the Linux infrastructure to the library director and users.


# VM Server side

## Used Ubuntu server

During the installation they asked me if I wanted to install OpenSSH.

I wanted/needed to put a fixed IP to my server:

```bash
cd /etc/netplan
vim 00-installer-config.yaml

network:
    ethernets: 
        eth0:
            dhcp4: False
            addresses: [192.168.1.1/24]
        eth1:
            dhcp4: True
netplan apply

## DHCP Server

Since "isc-dhcp-server" was no longer supported by his vendors and been replaced by Kea, I decided to use Kea for this project.

apt-get install kea
cd /etc/kea/
mv kea-dhcp4.conf kea-dhcp4.bu
touch kea-dhcp4.conf
vim kea-dhcp4.conf

{
    "Dhcp4": {
        "interfaces-config": {
            "interfaces": ["eth0"]
        },
        "control-socket": {
                "socket-type": "unix",
                "socket-name": "/run/kea/kea4-ctrl-socket"
        },
        "lease-database": {
            "type": "memfile",
            "lfc-interval": 3600
        },
        "valid-lifetime": 600,
        "max-valid-lifetime": 7200,
        "subnet4": [
            {
                "id": 1,
                "subnet": "192.168.1.0/24",
                "pools": [ 
                    {
                        "pool": "192.168.1.30 - 192.168.1.54"
                    }
                ],
                "option-data": [
                    {
                        "name": "routers",
                        "data": "192.168.1.254"
                    },
                    {
                        "name": "domain-name-servers",
                        "data": "192.168.1.1, 192.168.1.2"
                    },
                    {
                        "name": "domain-name",
                        "data": "mydomain.example"
                    }
                ]
                
            }
        ]
    }
}
systemctl restart kea-dhcp4-server.service
systemctl status kea-dhcp4-server.service

# DNS Server Configuration

## Step 1: Install Bind9

```bash
sudo apt-get update
sudo apt-get install -y bind9

cd /etc/bind
sudo vim named.conf.options

forwarders {
    8.8.8.8;
};

sudo systemctl stop systemd-resolved
sudo systemctl disable systemd-resolved

