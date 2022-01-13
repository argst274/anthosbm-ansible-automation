# Ansible Automation for Installing Anthos Bare Metal Intel Nuc Demo 

#Shout out to Matthew Winter whos code set the foundation for this deployment

#This demo is only built for Ubuntu hosts

Arhictecture:

```ditaa {cmd=true args=["-E"]}
+--------------------------+    +-------------------------------+
| Chromebook               |    |Ubuntu 20.10 Desktop           |
| Ansible Control Machine  +--->|Box1/Anthos Workstation/Proxy  |
| wlan: 192.168.1.XX       |    |wlan: 192.168.1.13 (Static)    |
|                          |    |eth:  10.1.1.10 (Static)       |
+--------------------------+    +---------------+---------------+
                                                |
                                         +------v-----+
                                         | eth switch |
                           +-------------+------------+-------------+
                           |                                        |
                           |                                        |
                           |                                        |
           +---------------v---------------+       +----------------v--------------+
           |Ubuntu 20.10 Server            |       |Ubuntu 20.10 Server            |
           |Box2/Anthos Cluster            |       |Box3/Anthos Cluster            |
           |eth:  10.1.1.11 (DHCP)         |       |eth:  10.1.1.12 (DHCP)         |
           |                               |       |                               |
           +-------------------------------+       +-------------------------------+
  ```

1. Install the latest version of Ansible on your Control Machine.
```
pip3 install ansible
```
2. Setup Control Machine
Install Ansible Galaxy collections (Options 1),
Install Google Cloud SDK on your Control Machine (Options 2), and
Initialise Google Cloud Login on your Control Machine (Option 3
```
./run 
```
3. Deploy Anthos Workstation/Box1/Proxy 
 
Install ubuntu desktop minimal on box1 enable ssh and add the ansible runner account with passwordless sudo. 
```
sudo apt update -y  && sudo apt install openssh-server -y
sudo ufw allow openssh
sudo adduser ansible-runner
sudo rm -f /etc/sudoers.d/*
cat <<EOF | sudo tee /etc/sudoers.d/00-ansible-runner
ansible-runner   ALL=(ALL) NOPASSWD:ALL
EOF
```
Create ssh key pair and copy public key to box1
```
ssh-copy-id -i ~/.ssh/id_ed25519.pub ansible-runner@<host>
```
Configure static IP for private network interface on box1,
update ansible variables file and
install Anthos Workstation (Option 4) 
```
./run 
```
4. Deploy Anthos Cluster
Install ubunutu server on box2 and box3 enable ssh and add the ansible runner account as above.
```
sudo apt update -y  && sudo apt install openssh-server -y
sudo ufw allow openssh
sudo adduser ansible-runner
sudo rm -f /etc/sudoers.d/*
cat <<EOF | sudo tee /etc/sudoers.d/00-ansible-runner
ansible-runner   ALL=(ALL) NOPASSWD:ALL
EOF
```
Create ssh key pair and copy public key to box2 & box 3
```
ssh-copy-id -i ~/.ssh/id_ed25519.pub ansible-runner@<host>
```
Install Anthos Cluster (Option 5) 
```
./run 
```


  Troubleshooting
  - Ensure that constraints/iam.disableServiceAccountKeyCreation policy is not enforced