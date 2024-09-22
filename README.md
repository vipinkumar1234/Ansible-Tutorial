Installing and configuring Ansible on Ubuntu EC2 instance: 


Step 1: Install Ansible on Master EC2 Instance

# Commands
1. sudo apt-get update, 2. sudo apt-get install -y software-properties-common 3. sudo apt-add-repository  ppa:ansible/ansible 4. sudo apt update 5. sudo apt install ansible 6. ansible --version

Step 2: on Worker 

sudo apt-get update
sudo apt-get install python

Step 3: Configure SSH Key-Based Authentication

On Master: ssh-keygen  

id_ed25519.pub

on Slave: sudo nano .ssh/authorized_keys 

Then add the key

4. Test SSH connection: from master 

ssh ubuntu@public_ip

Step 5: Configure Ansible Inventory

sudo nano /etc/ansible/hosts

[Prodservers]
worker1 ansible_host=43.204.141.229 ansible_user=ubuntu

Step 6: Run a test
ansible Prodservers -m ping

ansible all -m ping

Step 7: Install Required Software on Slave

ansible all -m apt -a "name=nginx state=present" --become


Creating Playbook

Step 1: Create a Playbook

sudo nano install_apache.yml

---
- name: Install Apache on Worker EC2
  hosts: Prodservers
  become: yes
  tasks:
    - name: Update apt package manager
      apt:
        update_cache: yes

    - name: Install Apache2
      apt:
        name: apache2
        state: present


Step 2: Run the Playbook

ansible-playbook install_apache.yml

Once the playbook runs successfully, you can verify the Apache installation by visiting the Worker EC2 instance's public IP address in a browser: