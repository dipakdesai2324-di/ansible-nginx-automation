# ansible-nginx-automation
Automate Nginx installation and Linux web server configuration using Ansible playbooks, inventory, and SSH.

## Project Overview
This project demonstrates how to automate Nginx installation and web-server configuration on Ubuntu Linux servers using Ansible.
Ansible connects to target servers through SSH and executes tasks defined in YAML playbooks.

## Objectives
* Automate Nginx installation.
* Manage the Nginx service.
* Deploy a custom HTML page.
* Verify the web server response.
* Demonstrate repeatable and idempotent configuration management.

## Technologies Used
* Ansible
* Ubuntu Linux
* SSH
* YAML
* Nginx
* Git and GitHub

## Project Structure
```text
ansible-nginx-automation/
├── README.md
├── ansible.cfg
├── inventory/
│   └── hosts.ini
├── playbooks/
│   └── install-nginx.yml
├── files/
│   └── index.html
└── .gitignore
```

## Prerequisites
* Ansible installed on the control node.
* An Ubuntu target server reachable over SSH.
* A valid SSH private key kept securely on the control node.
* Sudo privileges on the target server.
* Network access for installing Nginx.
* HTTP access to the server for browser testing.

## Configuration
Update `inventory/hosts.ini` with your target server's public IP, SSH username, and private-key path.
Ensure that the server's firewall or cloud security group allows SSH (TCP port 22) from your control node. Allow HTTP (TCP port 80) from your intended client IP or network for browser testing.
Do not commit private keys or credentials to GitHub.

## Execution
### 1. Verify Ansible installation
ansible --version
### 2. Test SSH connectivity
ansible webservers -m ansible.builtin.ping
### 3. Validate the inventory
ansible-inventory --list
### 4. Check playbook syntax
ansible-playbook --syntax-check playbooks/install-nginx.yml
### 5. Run the playbook
ansible-playbook playbooks/install-nginx.yml
### 6. Verify Nginx
Open the target server's public IP address in a browser:
http://YOUR_SERVER_PUBLIC_IP
The custom webpage should appear if the service is running and HTTP traffic is permitted.

### 7. Run the playbook again
Execute the playbook a second time to observe Ansible idempotency. If the server already matches the desired configuration, most tasks should report `ok` rather than `changed`.

## Expected Outcome
* Nginx is installed on the target server.
* Nginx starts and is enabled at boot.
* The custom HTML page is deployed.
* The playbook verifies the local HTTP response.
* Repeated execution maintains the desired configuration without unnecessary changes.

## Learning Outcomes
This project provides hands-on practice with Ansible inventories, playbooks, YAML, SSH connectivity, privilege escalation, service management, file deployment, and idempotent automation.
