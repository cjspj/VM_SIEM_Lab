# Build a SIEM
My goal for this project is to document, configure, modify, and manage a SIEM (Security information and event management), in a homelab enviroment. As well as implement cybersecurity tools to practice with. 
- VMware Workstation Pro running Ubuntu 22 and with Kali Linux 
- Setup, update, and configure Ubuntu to have the SIEM running 24/7
- Setup and configure Wazuh
- Deploy Kali Linux as an agent
- Deploy Windows 10 and MacOS as agents
- Improve, update, configure, and continuously monitor events

#  Step 1

Download VMware , Ubuntu 22 ISO
-https://www.vmware.com/products/workstation-player/workstation-player-evaluation.html
-https://ubuntu.com/download/desktop
-https://www.kali.org/get-kali/#kali-virtual-machines (VMware)

Go through virtual machine wizard.
- input username / password
- customize hardware, 4gb 2 cpu cores
- new install, minimal installation, download updates while install
- Fresh install
- Open up terminal

  sudo apt install update
  sudo apt install upgrade
  sudo apt install curl
  sudo apt install net-tools
  sudo apt install tar 


#  Step 2 Wazuh install

Installing the Wazuh indexer using the assisted installation method

1. Download the Wazuh installation assistant and the configuration file.
   
   curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh
   curl -sO https://packages.wazuh.com/4.7/config.yml

2. Edit ./config.yml
   
in the terminal 

  sudo nano config.yml
   
   and replace the node names and IP values with the corresponding names and IP addresses.
   You need to do this for all Wazuh server, Wazuh indexer, and Wazuh dashboard nodes. Add as many node fields as needed.

ifconfig on terminal and find your IP address copy and paste the ip into the indexer, server, and dashboard 

<indexer-node-ip>, <wazuh-manager-ip>,  <dashboard-node-ip>

_______________________________________________________________________________________

   nodes:
  # Wazuh indexer nodes
  indexer:
    - name: node-1
      ip: "<indexer-node-ip>"
    #- name: node-2
    #  ip: "<indexer-node-ip>"
    #- name: node-3
    #  ip: "<indexer-node-ip>"

   # Wazuh server nodes
   # If there is more than one Wazuh server
    # node, each one must have a node_type
  server:
    - name: wazuh-1
      ip: "<wazuh-manager-ip>"
    #  node_type: master
    #- name: wazuh-2
    #  ip: "<wazuh-manager-ip>"
    #  node_type: worker
    #- name: wazuh-3
    #  ip: "<wazuh-manager-ip>"
    #  node_type: worker

  # Wazuh dashboard nodes
  dashboard:
    - name: dashboard
      ip: "<dashboard-node-ip>"
_______________________________________________________________________________________
3. Generate install files.
   
sudo bash wazuh-install.sh --generate-config-files

4. Install the Wazuh server

sudo bash wazuh-install.sh -a

It took about 8 minutes to complete installation
Copy the admin and password and paste to notes, change defaults later on.

Open terminal ifconfig, copy ip address.
Open firefox input into url bar https://"ip address"
Advanced > Accept risk and continue 

#  Step 3 Add an agent

Configure new VM > fire up kali linux
Home > add agent > select package > deb amd64 > server address: "IP address" > assign agent name: kali_vm

1.  Run the following commands to download and install the agent:

wget https://packages.wazuh.com/4.x/apt/pool/main/w/wazuh-agent/wazuh-agent_4.7.2-1_amd64.deb && sudo WAZUH_MANAGER='IP ADDRESS' 
WAZUH_AGENT_GROUP='default' WAZUH_AGENT_NAME='kali_vm' dpkg -i ./wazuh-agent_4.7.2-1_amd64.deb

  
Open terminal in kali and paste above ^ 

2.  Start the agent:

sudo systemctl daemon-reload
sudo systemctl enable wazuh-agent
sudo systemctl start wazuh-agent

3. wazuh passwords

   cd wazuh-install-files/
   sudo cat wazuh-passwords.txt | more




# Problems / Fixes so far

- Killed process in windows  'mksSandbox.exe' resulting VMware not having a network connection with the host.
- FIX: restarting windows resolved the problem.
- Issue with not being able to extract wazuh-files.tar.
- FIX:
  su root tar xfv wazuh-install-files.tar
