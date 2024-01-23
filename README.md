# Description
My goal for this project is to get first hand experience setting up a VM (Virtual Machine) and utilizing a SIEM (Security information and event management) in a homelab enviroment. I will document, modify, configure and use this as a reference if I ever need to setup and configure another instance.
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

  sudo apt install update
  
  sudo apt install upgrade
  
  sudo apt install curl
  
  sudo apt install net-tools
  
  sudo apt install tar
_____________________________________________________________________________________________________________________________________________________________________________________________________________________
#  Wazuh
-  Open firefox in Ubuntu
-  Type in URL: https://documentation.wazuh.com/current/installation-guide/wazuh-indexer/installation-assistant.html

   curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
   curl -sO https://packages.wazuh.com/4.7/config.yml

In terminal:
  sudo nano config.yml
  
![image](https://github.com/cjspj/VM_SIEM_Lab/assets/90308312/edd78d2b-0356-426f-ba3d-4d6d4a5d8703)


-  On the terminal find the ip address associated with the VM
-  use command: ip addr 
-  replace the node names and IP values with the corresponding names and IP addresses
-  save modified buffer and exit 
________________________________________________________________________________________________________________________________________________________________________________________________________________
#  Generate install files

In terminal: 
sudo bash wazuh-install.sh --generate-config-files
  
In terminal:
sudo bash wazuh-install.sh -a

-  8 minutes to complete installation
-  Copy the password and paste to notes, change defaults later on.

  ![image](https://github.com/cjspj/VM_SIEM_Lab/assets/90308312/7eb2ea22-55e5-4112-84b9-38db4fc023c9)

In terminal:
-  ifconfig: copy ip address
-  Open firefox input into url bar https://"ip address"
-  Advanced > Accept risk and continue
-  Enter admin and password

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
In Ubuntu terminal: 

su root tar xfv wazuh-install-files.tar

cd wazuh-install-files/

sudo cat wazuh-passwords.txt | more

- Save any passwords

![image](https://github.com/cjspj/VM_SIEM_Lab/assets/90308312/f75a6559-1d0e-4844-8c07-64fef320d65c)

If everything goes right I should have the kali_vm agent active and functional. 
_____________________________________________________________________________________________________________________________________________________________________________________________________________________
# Final Thoughts / Future Addons

-  Deploy Windows and MacOS as an agent
-  Setup for 24/7 home network
-  Monitor and log events 







