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
