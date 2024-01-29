# Description
In this project, my primary objective is to gain firsthand experience in the setup of a Virtual Machine (VM) and the effective utilization of a Security Information and Event Management (SIEM) system within a homelab environment. I aim to meticulously document, modify, and configure this setup, creating a comprehensive reference guide for future instances where I need to establish and configure similar environments. This project serves as both a learning journey and a practical resource for seamless VM and SIEM implementation.
_____________________________________________________________________________________________________________________________________________________________________________________________________________________
# Tools 
- Lenovo ThinkPad i7-8550U 32GB
- Windows 11 Pro
- VMware Workstation Pro
- Ubuntu 
- Kali Linux
- Wazuh
_____________________________________________________________________________________________________________________________________________________________________________________________________________________
#  Downloads
-  Ubuntu:  https://ubuntu.com/download/desktop
-  VMware:  https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html
-  Kali Linux:  https://www.kali.org/get-kali/#kali-virtual-machines (VMware)
-  Wazuh:  https://documentation.wazuh.com/current/installation-guide/wazuh-indexer/installation-assistant.html
_____________________________________________________________________________________________________________________________________________________________________________________________________________________
# Installation 
![image](https://github.com/cjspj/VM_SIEM_Homelab/assets/90308312/d4664cd0-0072-490a-8bd3-c74981ffd298)
-  Install Ubuntu, have the .iso file ready to mount 
-  Install VMware, run it
-  New Virtual Machine Wizard > Browse and find the Ubuntu .iso file
-  Enter user credentials for login
-  Customize hardware > 20GB disk capacity space > RAM: 8GB > Processor cores: 2
-  Start up guest > run the setup > select the minimal installation options > username, password >
_____________________________________________________________________________________________________________________________________________________________________________________________________________________
#  Ubuntu
-  Once installation is complete
-  Run the following commands in terminal:

  ![image](https://github.com/cjspj/VM_SIEM_Lab/assets/90308312/446c3c42-8c09-436c-a618-eead7abe7603)
```<br><br>
  sudo apt install update
  
  sudo apt install upgrade
  
  sudo apt install curl
  
  sudo apt install net-tools
  
  sudo apt install tar
```
<br><br>
______________________________________________________________________________________________________________________________________________________________________________________________________
#  Wazuh
-  Open firefox in Ubuntu
-  Type in URL: https://documentation.wazuh.com/current/installation-guide/wazuh-indexer/installation-assistant.html

   curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
   curl -sO https://packages.wazuh.com/4.7/config.yml

In terminal:
  sudo nano config.yml [`config.yml`](config.yml)
  
![image](https://github.com/cjspj/VM_SIEM_Lab/assets/90308312/edd78d2b-0356-426f-ba3d-4d6d4a5d8703)

# Run the following command to view the IP addresses
ip addr

# Identify the relevant IP address associated with the VM

# Open a text editor (e.g., nano) to modify the buffer
nano /etc/hosts

# Replace the existing node names and IP values with the corresponding names and IP addresses

# Save the modified buffer and exit the text editor
# In nano, you can typically press Ctrl + X, then press Y to confirm changes, and finally press Enter to exit

________________________________________________________________________________________________________________________________________________________________________________________________________________
#  Generate install files

In terminal: 
sudo bash wazuh-install.sh --generate-config-files
  
In terminal:
sudo bash wazuh-install.sh -a

-  8 minutes to complete installation
-  Copy the password and paste to notes, change defaults later on.

  ![image](https://github.com/cjspj/VM_SIEM_Lab/assets/90308312/7eb2ea22-55e5-4112-84b9-38db4fc023c9)

# Run ifconfig to retrieve the IP address
ifconfig

# Copy the IP address displayed in the terminal

# Open Firefox and enter the Wazuh manager URL
firefox https://<IP_ADDRESS>

# In Firefox, navigate to "Advanced," then click "Accept the Risk and Continue"

# Enter the default credentials
# Username: admin
# Password: <password>

# You should now have access to the Wazuh manager interface.


_____________________________________________________________________________________________________________________________________________________________________________________________________________________
# Kali Linux + Deploy agent

![image](https://github.com/cjspj/VM_SIEM_Lab/assets/90308312/f60f411f-7fd1-435c-ac82-728f29588e45)

- Power on the virtual machine
- Minimal configuration setup 

- Home > Add agent > Select package > deb amd64 > server address: VM IP > assign agent name: kali_vm > default: default
- Run the following download and commands to run the agent

Copy from Windows to:
In kali_vm terminal:

sudo wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.7.2-1_amd64.deb && sudo WAZUH_MANAGER='IP ADDRESS' 
WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='kali_vm' dpkg -i ./wazuh-agent_4.7.2-1_amd64.deb

sudo systemctl daemon-reload

sudo systemctl enable wazuh-agent

sudo systemctl start wazuh-agent
_____________________________________________________________________________________________________________________________________________________________________________________________________________________
# Switch to root user
su root

# Extract Wazuh installation files
tar xfv wazuh-install-files.tar

# Move to the installation directory
cd wazuh-install-files/

# View Wazuh passwords
sudo cat wazuh-passwords.txt | more

# Save any relevant passwords

# Check installation progress
# [Optional] You can verify the installation status by viewing the provided screenshot
![Installation Progress](https://github.com/cjspj/VM_SIEM_Lab/assets/90308312/f75a6559-1d0e-4844-8c07-64fef320d65c)

# If everything goes as expected, the Kali Linux VM agent should be active and fully functional.

_____________________________________________________________________________________________________________________________________________________________________________________________________________________
# Final Thoughts / Future Addons

-  Deploy Windows and MacOS as an agent
-  Setup for 24/7 home network
-  Monitor and log events 







