# Ansible Automation for Installing Anthos Bare Metal Intel Nuc Demo 

#Shout out to Matthew Winter whos code set the foundation for this deployment

#This demo is only built for Ubuntu hosts


1. Install latest version of Ansible on your Control Machine. 
```
pip3 install ansible
```
2. Install ubunutu desktop on box1 enable ssh and add the ansible runner account
```
sudo apt update -y  && sudo apt install openssh-server -y
sudo ufw allow openssh
sudo adduser ansible-runner
sudo rm -f /etc/sudoers.d/*
cat <<EOF | sudo tee /etc/sudoers.d/00-ansible-runner
ansible-runner   ALL=(ALL) NOPASSWD:ALL
EOF
```
3. Install ubunutu server on box2 and box3 enable ssh and add the ansible runner account as above.

4. Create ssh key pair and copy public key to target hosts
```
ssh-copy-id -i ~/.ssh/id_ed25519.pub ansible-runner@<host>
```
5. Connect to your local wifi and configure static IP for your pysical ethernet  (10.1.1.10)

6. Update variables

7. Execute run script
```
./run
```

The following digram shows my environment

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

  Troubleshooting
  - Ensure that constraints/iam.disableServiceAccountKeyCreation policy is not enforced