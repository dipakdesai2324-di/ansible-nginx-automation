# How to Run the Ansible Nginx Automation Project

## 1. Project Overview
This project demonstrates how to use Ansible to automate Nginx installation and web-server configuration on an Ubuntu Linux server.
Ansible connects to the target server through SSH and executes tasks defined in a YAML playbook.
The playbook performs the following operations:
* Installs Nginx.
* Starts the Nginx service and enables it at boot.
* Deploys a custom HTML webpage.
* Verifies that the web server responds successfully.

## 2. Prerequisites
Before executing the project, ensure that the following requirements are met:
* Ansible is installed on the control node.
* An Ubuntu target server is available.
* SSH connectivity between the control node and target server is configured.
* A valid SSH private key is available on the control node.
* The SSH user has appropriate sudo privileges.
* The target server has internet access to install Nginx.
* Required network rules permit SSH and HTTP traffic.
**Note:** On a Windows laptop, Ubuntu running through WSL 2 can be used as the Ansible control node.

## 3. Install Ansible
Open the Ubuntu terminal on the control node.
Update the package index: sudo apt update
Install Ansible: sudo apt install ansible -y
Verify the installation: ansible --version
The command should display the installed Ansible version and configuration details.

## 4. Configure the Inventory
Open the following file in the project: inventory/hosts.ini
Configure the target server details:
[webservers]
web1 ansible_host=YOUR_SERVER_PUBLIC_IP ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/your-key.pem
Replace the placeholders with the actual values:
* YOUR_SERVER_PUBLIC_IP: Public IP address of the target Ubuntu server.
* ubuntu: SSH username for the target server.
* ~/.ssh/your-key.pem: Path to the SSH private key on the control node.
The inventory tells Ansible which servers belong to the `webservers` group and how to connect to them.
Do not upload your private SSH key to GitHub.

## 5. Configure SSH Key Permissions
If the private key is stored in the default SSH directory, set its permissions: chmod 400 ~/.ssh/your-key.pem
Replace your-key.pem with the actual key filename.
Test SSH connectivity: ssh -i ~/.ssh/your-key.pem ubuntu@YOUR_SERVER_PUBLIC_IP
Replace the key path, username, and public IP with your actual values.
If the connection succeeds, exit the SSH session: exit

## 6. Navigate to the Project Directory
Open the Ubuntu terminal and navigate to the directory containing the project.
For example:  cd ~/ansible-nginx-automation
Use the actual path where you cloned or downloaded the repository.
If the project is not yet available in Ubuntu, clone your GitHub repository:  git clone YOUR_GITHUB_REPOSITORY_URL
cd ansible-nginx-automation
Replace `YOUR_GITHUB_REPOSITORY_URL` with the repository's actual HTTPS or SSH clone URL.

## 7. Verify Ansible Connectivity
Execute the following command: ansible webservers -m ansible.builtin.ping
This command checks whether Ansible can connect to the target servers and execute a basic module.
Expected output includes:
web1 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
The pong response indicates that Ansible successfully communicated with the target server.
If connectivity fails, check the server IP, SSH username, private-key path, SSH access rules, and user permissions.

## 8. Validate the Playbook
Before executing the playbook, check its syntax:  ansible-playbook --syntax-check playbooks/install-nginx.yml
If the syntax is valid, Ansible reports that the playbook syntax is okay.
This check validates the playbook's syntax but does not guarantee that every task will succeed during execution.

## 9. Execute the Playbook
Run the following command from the project root directory:  ansible-playbook playbooks/install-nginx.yml
Ansible executes the tasks defined in the playbook against the servers in the `webservers` inventory group.
The playbook performs the following operations:
1. Installs Nginx using the APT package manager.
2. Starts the Nginx service.
3. Enables Nginx to start automatically when the server boots.
4. Copies `files/index.html` to `/var/www/html/index.html`.
5. Checks whether Nginx returns HTTP status code `200`.
6. Displays a success message when verification passes.
The playbook uses privilege escalation through `become: true`, so the SSH user must have the required sudo permissions.

## 10. Verify the Application
After the playbook completes successfully, open a browser and navigate to:http://YOUR_SERVER_PUBLIC_IP
Replace YOUR_SERVER_PUBLIC_IP with the public IP address of the target server.
The custom webpage should display: **Nginx Deployed Using Ansible**
The page also explains that the web server was configured and deployed through automation.
If the page does not load, verify that Nginx is running and that the target server's firewall or cloud security group permits inbound HTTP traffic on TCP port 80.

## 11. Verify the Nginx Service
Connect to the target server through SSH and execute:  sudo systemctl status nginx
This command displays the current status of the Nginx service.
To verify the HTTP response directly on the target server:  curl -I http://127.0.0.1
A successful response should include:  HTTP/1.1 200 OK

## 12. Test Ansible Idempotency
Execute the playbook again:  ansible-playbook playbooks/install-nginx.yml
Ansible compares the existing server configuration with the desired configuration described in the playbook.
If the server already matches the desired state, most tasks should report `ok` instead of `changed`.
This demonstrates **idempotency**: repeatedly applying the same configuration should not make unnecessary changes when the desired state has already been reached.

## 13. Troubleshooting
### SSH Connection Failure
Check the public IP address, SSH username, private-key path, key permissions, and inbound SSH rules.
### Permission Denied
Verify that the SSH key belongs to the correct server and that the specified SSH user is authorized to connect.
### Nginx Installation Failure
Verify that the target server has internet access and that the APT package index can be updated.
### HTTP Page Not Loading
Check the Nginx service: sudo systemctl status nginx
Verify that inbound TCP port 80 is permitted by the server's firewall and cloud security group.

### Playbook Syntax Error
Run:  ansible-playbook --syntax-check playbooks/install-nginx.yml
Review the reported file and line number, then correct the YAML indentation or task configuration.

## 14. Clean Up
If you created an EC2 instance specifically for this project, stop or terminate it when it is no longer required, according to your cost and retention needs.
Before terminating the instance, ensure that you no longer need its data or configuration. Check for other billable resources, such as attached storage and allocated public IPv4 addresses.

## 15. Expected Outcome
After completing this project:
* Ansible connects to the target Ubuntu server over SSH.
* Nginx is installed and running.
* The Nginx service is enabled at boot.
* A custom HTML webpage is deployed.
* The HTTP response is verified.
* Repeated playbook execution demonstrates idempotent configuration management.

This project provides hands-on experience with Ansible inventories, playbooks, SSH connectivity, privilege escalation, package installation, service management, file deployment, and configuration automation.
