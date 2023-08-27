# Proxmox Installation on Bare Metal

### Prerequisites:
- Just your Old PC !
- Check [minimum requirments](https://www.proxmox.com/en/proxmox-virtual-environment/requirements)
### Step by step guide:
### 1. Download the ISO image for your operating system from [Proxmox](https://www.proxmox.com/en/downloads/proxmox-virtual-environment)

### 2. Prepare Bootable Media:
- Create a bootable USB drive or burn the ISO to a DVD using software like Rufus or Etcher.
### 3. Boot into Installation:
- Insert the bootable USB drive or DVD into the old PC.
- Boot from the media, and you'll see the Proxmox boot menu. Select "Install Proxmox VE" and press Enter.
### 4. Proxmox Installer:
- The installer will load. Choose your preferred language and click "Next."
- Accept the license agreement and click "Next."
### 5. Target Disk Selection:
- Select the disk where you want to install Proxmox.
- Choose whether to use the entire disk or set up partitions manually.
- Click "Next."
### 6. Network Configuration:
- Configure your network settings, including the hostname, IP address, netmask, gateway, and DNS.
- Click "Next."
### 7. Timezone and Root Password:
- Choose your timezone and set the root password.
- Click "Next."
### 8. Account Setup:
- Set up an admin account by providing a username, email, and password.
- Click "Next."
### 9. Package Repository:
- Choose the "No Subscription" option (unless you have a Proxmox subscription) and click "Next."
### 10. Installation Summary:
- Review your settings on the summary page.
- Click "Install" to start the installation process.
### 11. Installation Progress:
- The installer will copy files and configure the system. This may take a few minutes.
*You could think it's stuck but it's okay just give it some time.*
### 12. Reboot:
- Once the installation is complete, click "Reboot."
### 13. Login to Proxmox Web Interface:
- Open a web browser on another device and enter the IP address you configured during installation followed by port 8006 (e.g., https://your_server_ip:8006).
- You'll see a security warning; proceed to the interface.
- Log in with the admin account you created during installation.
### 14. Configuring your Proxmox Storage for your VMs and Containers
- For video refrence i advice you to watch Network Chuck's [Video](https://www.youtube.com/watch?v=_u8qTN3cCnQ&t=1599s&ab_channel=NetworkChuck)

## Finally, Your Proxmox is READY!
### It should be somthing like this:
![Alt text](Images\image.png)