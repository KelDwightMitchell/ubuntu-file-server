# **Ubuntu File Server Project**
## Objective:
To deploy Ubuntu Desktop on a repurposed Toshiba Satellite C55 laptop as a local network accessible file server. The server will use Samba for file sharing compatibility with Windows and OpenSSH for secure remote administration from a Windows 11 client machine, utilizing SSH keys over passwords for enhanced security.
## Environment:
- **Server Hardware:** Toshiba Satellite C55, 4GB DDR3 ram, Seagate Barracuda HDD
- **Client Machine:** Windows 11 Laptop
- **Server OS:** Ubuntu Desktop 24.04.4 LTS
- **Purpose:** Hands-on Linux server administration and remote management.
## Skills Demonstrated:
- Linux installation and configuration
- Static IP addressing
- Samba file server setup
- SSH key based authentication
- Remote server administration from windows
## Project Steps:
### Step 1 - Ubuntu Desktop installation
**Method:** Booted from a Ventoy multi-boot USB containing the Ubuntu Desktop ISO image.
**Process:** 
- Inserted Ventoy USB, booted up into The Toshiba Satellite C55's BIOS, changed the boot order/priority to the Ventoy USB and booted into Ventoy
- Selected Ubuntu Desktop ISO from the Ventoy boot menu
- Followed the Ubuntu installation wizard
- Created a local user account during setup
- Completed installation and rebooted into Ubuntu Desktop

**Outcome:** Ubuntu Desktop successfully installed and booting normally.
**OS selected:** Ubuntu Desktop 24.04.04 LTS chosen for its long term 
support stability and suitability for a server environment.
