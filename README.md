# Example Ansible Automation for Installing Anthos Bare Metal


## Setup Remote User
Make sure that the remote user account is configured to allow for the execution of sudo without the need to enter a password.  An example of how this can be achieved can be found below:

```
# Setup sudoers for remote user account e.g "ansible-runner"
sudo rm -f /etc/sudoers.d/*
cat <<EOF | sudo tee /etc/sudoers.d/00-ansible-runner
ansible-runner   ALL=(ALL) NOPASSWD:ALL
EOF
```

## Ansible Control Machine Setup
On your Ansible Control Machine, ensure that you install and initialize the Google Cloud SDK using these [instructions](https://cloud.google.com/sdk/docs). This process will install gcloud and gsutil.

Next we need to loging in with your Google Account which will be used by Ansible to manage the services and service accounts:
```
gcloud auth login --update-adc
```
and finally ensure that you setup the default Google Cloud Project
```
gcloud config set project "PROJECT_ID"
```


1. Install latest version of Ansible on Control Machine. 
```
pip3 install ansible
```
2. Install ubunutu desktop on box 1 enable ssh and add the ansible runner account
```
sudo apt update -y  && sudo apt install openssh-server -y
sudo adduser ansible-runner
sudo rm -f /etc/sudoers.d/*
cat <<EOF | sudo tee /etc/sudoers.d/00-ansible-runner
ansible-runner   ALL=(ALL) NOPASSWD:ALL
EOF
```

3. Copy create ssh key pair and copy public key to box1
```
ssh-copy-id -i ~/.ssh/id_ed25519.pub anisble-runner@<box2-ip>
```
3. Update hosts and variables

4. Execute run script
```
./run
```
