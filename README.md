# HSD Community Nodes

This repository contains ansible playbooks to deploy the hsd on nodes provided by the community.
In other words, this repository is used to deploy and maintain the hsd on the nodes that are provided by the community.

## How to collaborate:

1. Create a new server instance in your preferred cloud provider.
2. Add our [public key](./ssh_key.pub) to the authorized_keys file.
3. Use [this GitHub Action](https://github.com/Falci/hsd-community-nodes/actions/workflows/update-inventory.yaml) to add the server to the inventory.
   - You will need to provide the IP address and the username of the server.

From there, we will check if the server has the necessary requirements to run the hsd and if it does, we will deploy the hsd on the server.
Once there are updates, we will update the hsd on all the servers.

## Step by step

### Create a new server instance

Login to your preferred cloud provider and create a new server instance.
We recommend Linode ([affiliate link](https://s.falci.me/linode)).

TBD: recommendations for the server instance.

It must have at least 40GB of disk space.

### Add the public key to the authorized_keys file

Add the [public key](./ssh_key.pub) to the authorized_keys file of the server.

```bash
curl -s https://raw.githubusercontent.com/Falci/hsd-community-nodes/refs/heads/master/ssh_key.pub >> ~/.ssh/authorized_keys
```

Note: This will grant us access to the server. By adding the public key to the authorized_keys file, you agree to give us access to the server.

### Add the server to the inventory

Use [this GitHub Action](https://github.com/Falci/hsd-community-nodes/actions/workflows/update-inventory.yaml) to add the server to the inventory.
It will ask for the IP address and the username of the server. Then, it will run some checks to see if the server has the necessary requirements to run the hsd.
If the server passes the checks, the same action will create a Pull Request to add the server to the inventory.
Please, don't create a Pull Request manually.

### Deploy the hsd

Once the Pull Request is merged, we will deploy the hsd on the server.
We will install all the necessary dependencies and configure the server to run the hsd.

### Updates

When there are updates to the hsd, we will update the hsd on all the servers.
For this, it's important to have the servers in the inventory and keep our key in the authorized_keys file.

## Test locally

```bash
# Optional:
export ANSIBLE_HOST_KEY_CHECKING=False

# Create a ssh key pair
ssh-keygen -t rsa -b 4096 -f ./keys/ansible_key -N ""

# Create an inventory file
echo "[all]" > ansible/inventory.test.ini
echo "<IP> ansible_user=root ansible_ssh_private_key_file=./keys/ansible_key"

# Run the ansible playbook
ansible-playbook -i ansible/inventory.test.ini ansible/<playbook>
```
