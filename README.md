# **Ubuntu File Server Project**
## Objective:
To deploy Ubuntu Desktop on a repurposed Toshiba Satellite C55 laptop as a local network accessible file server. The server will use Samba for file sharing compatibility with Windows and OpenSSH for secure remote administration from a Windows 11 client machine, utilizing SSH keys over passwords for enhanced security.
## Environment:
- **Server Hardware:** Toshiba Satellite C55, 4GB DDR3 RAM, Seagate Barracuda HDD
- **Client Machine:** Windows 11 Laptop
- **Server OS:** Ubuntu Desktop 24.04.4 LTS
- **Purpose:** Hands-on Linux server administration and remote management.
## Skills Demonstrated:
- Linux installation and configuration
- Static IP addressing
- Samba file server setup
- SSH key based authentication
- Remote server administration from Windows
## Project Steps:
### Step 1 - Ubuntu Desktop Installation
**Method:** Booted from a Ventoy multi-boot USB containing the Ubuntu Desktop ISO image.

**Process:** 
- Inserted Ventoy USB, booted up into the Toshiba Satellite C55's BIOS, changed the boot order/priority to the Ventoy USB and booted into Ventoy
- Selected Ubuntu Desktop ISO from the Ventoy boot menu
- Followed the Ubuntu installation wizard
- Created a local user account during setup
- Completed installation and rebooted into Ubuntu Desktop

**Outcome:** Ubuntu Desktop successfully installed and booting normally.

**OS selected:** Ubuntu Desktop 24.04.04 LTS chosen for its long term 
support stability and suitability for a server environment.

### Step 2 - Service Installation & System Optimization
After updating all system and necessary services packages, the server was tuned to address the hardware limitations of a mechanical HDD and 4GB of RAM. This "System Hardening" phase ensures the file server remains responsive under load by specifically optimizing memory management to prevent excessive disk swapping. Resulting in heightened availability and smoother performance.

**Services Installed:**
- **OpenSSH & Samba:** Installed to allow remote CLI management and cross-platform file sharing.
- **Verification**:
```Bash
systemctl status ssh smbd
# Should show active (running) and (enabled)
```
![SSH & Samba running and enabled](screenshots/samba-ssh%20verified.png)

**Hardware Optimizations:**
- **ZRAM Implementation:** A compressed RAM swap was initialized using the **LZ4 algorithm.** This creates a high-speed buffer, preventing the system from "thrashing" the slow physical HDD when memory usage spikes.
- **Validation:**
```Bash
 zramctl 
# Confirmed /dev/zram0 is active with 1.8G disk size
 ```
 ![ZRAM running](screenshots/zramctl.png)
- **Virtual Memory Optimization:** The *vm.swappiness* parameter was reduced from 60 to **10.** This instructs the kernel to prioritize physical RAM/ZRAM, only utilizing the HDD swap as a last resort.
- **Validation:** 
```Bash
cat /proc/sys/vm/swappiness 
# Output:10
```
![Successful swappiness](screenshots/swappiness.png)
- **Filesystem Metadata Reduction:** The root partition was remounted with the ***noatime*** flag. This eliminates unnecessary disk writes during file "read" operations, preserving HDD bandwidth for actual data transfers.
- **Validation:** 
```Bash
mount | grep " / " 
# Look for 'noatime' in the mount options parentheses
```
![Noatime validated](screenshots/noatime.png)
- **Background Indexing Deactivation:** The **Tracker3** suite (system-wide file indexing) was masked to reclaim more available RAM and stop background disk grinding.
- **Validation:**
```Bash
 systemctl --user list-unit-files | grep tracker 
 # Status should show 'masked' for all miner/extractor services
 ```
 ![All Tracker3 services masked](screenshots/tracker3-masked.png)
### **Resource Baseline (Post-Optimization)**
**Monitoring:** Utilized *htop* to establish a performance baseline, verifying that background I/O wait is minimized and ZRAM is handling memory pressure efficiently.

**Observed Results:**
- **RAM Usage:** 950MB/3.69GB at idle
- **Swap:** ZRAM active, minimal HDD swap utilization
- **CPU:** Low idle usage confirming background processes are minimal
- **I/O Wait:** Minimal, confirming noatime and Tracker3 masking are effective
![Resource Baseline - htop and ZRAM confirmation](screenshots/htop-zramctl.png)

**Conclusion:** The optimized configuration demonstrates efficient resource 
utilization suitable for sustained file server operation on legacy hardware.
