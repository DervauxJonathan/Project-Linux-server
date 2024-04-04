# Project-Linux-server

## Linux Infrastructure Setup

### Server Configuration

- **Operating System**: Linux (no GUI)
- **Services**:
  - DHCP: isc-dhcp-server
  - DNS: bind
  - HTTP + MariaDB: GLPI
- **Backup Strategy**:
  - Weekly backup of configuration files for each service into a single compressed archive.
  - Optional: Backups are placed on a partition located on a separate disk and then unmounted.

### Workstation Configuration

- **Operating System**: Linux with Desktop Environment
- **Applications**:
  - LibreOffice
  - Gimp
  - Mullvad browser
- **Networking**:
  - Automatic addressing for network configuration
- **File System**:
  - /home folder located on a separate partition on the same disk.
- **Remote Assistance Solution** (Optional):
  - Propose and implement a solution to remotely help a user.

### Additional Requirements

- **Remote Management**:
  - SSH for remote management of the server.
