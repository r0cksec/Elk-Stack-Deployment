## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.


![Virtual Network](https://github.com/r0cksec/Elk-Stack-Deployment/blob/master/Diagrams/ELK-Connection.jpg)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the __.yml__ file may be used to install only certain pieces of it, such as Filebeat.

  - ![Deploy Elk](https://github.com/r0cksec/Elk-Stack-Deployment/blob/master/Ansible/install-elk-playbook.yml)
  - ![Install Filebeat](https://github.com/r0cksec/Elk-Stack-Deployment/blob/master/Ansible/Filebeat/filebeat-playbook.yml)
  - ![Install Metricbeat](https://github.com/r0cksec/Elk-Stack-Deployment/blob/master/Ansible/Metricbeat/metricbeat-playbook.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly __accessible__, in addition to restricting __the amount of traffic__ to the network.
- Load balancers protect the aspect of accessibilty in security. The advantage of a jump-box is that the only users that can access the servers are the users that have access to the jumpbox.  

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the file system and system metrics.
- Filebeat watches for changes in files and generates logs on the file system.  
- Metricbeat records system metrics like uptime, CPU Usage, etc.  

The configuration details of each machine may be found below.

| Name     | Function       | IP Address | Operating System     |
|----------|----------------|------------|----------------------|
| Jump Box | Gateway        | 10.0.0.4   | Ubuntu 18.04 (Linux) |
| Web 1    | Host DVWA      | 10.0.0.5   | Ubuntu 18.04 (Linux) |
| Web 2    | Host DVWA      | 10.0.0.6   | Ubuntu 18.04 (Linux) |
| Web 3    | Host DVWA      | 10.0.0.7   | Ubuntu 18.04 (Linux) |
| Elk-VM   | Host ELK Stack | 10.1.0.4   | Ubuntu 18.04 (Linux) |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
-24.224.10.137 (Home-IP)

Machines within the network can only be accessed by accessing the jumpbox.
- I Allowed my Jumpbox to SSH into the ELK VM if in the ansible container. 40.78.28.127

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses             |
|----------|---------------------|----------------------------------|
| Jump Box | Yes                 | 24.244.10.137 (Home-IP)          |
| Web 1    | No                  | 10.0.0.4                         |
| Web 2    | No                  | 10.0.0.4                         |
| Web 3    | No                  | 10.0.0.4                         |
| Elk-VM   | Yes                 | 10.0.0.4 24.244.10.137 (Home-IP)
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- Automating configurations with Ansible allows for quick and easy deployment.  

The playbook implements the following tasks:
- Install docker-io with the apt module
- Install python3-pip with the apt module
- Install docker module with pip
- Set the Elk VM to use more memory with the sysctl module
- Download and launch the docker elk container. 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.


![Docker Container Status](https://github.com/r0cksec/Elk-Stack-Deployment/blob/master/Images/ELKContainer.PNG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6
- 10.0.0.7

We have installed the following Beats on these machines:
- Filebeat 
- Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat is used to collect system logs, some of the logs we could see are SSH Logins, Sudo commands, etc. 
- Metricbeat is used to collect system metrics, CPU Usage, Memory usage, etc.

### Using the Playbooks
In order to use the playbooks, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 
##### Filebeat
SSH into the control node and follow the steps below:
- Copy the __filebeat-config.yml__ file to __/etc/ansible/files/filebeat-config.yml__.

- Update the __filebeat-config.yml__ file to include your ELK Machines IP address under output.elasticsearch, and under    setup.kinbana. 
  ```bash
  output.elasticsearch:
  hosts: ["10.1.0.4:9200"]
  ```
   ```   
  setup.kibana:
  host: "10.1.0.4:5601"
  ```
- Run the playbook, and navigate to the Filebeat installation page on the ELK server GUI to check that the installation worked as expected by clicking the check data button
##### Metricbeat
SSH into the control node and follow the steps below:
- Copy the __metribeat-config.yml__ file to __/etc/ansible/files/metricbeat-config.yml__.
- Update the __metricbeat-config.yml__ file to include your ELK Machines IP address under output.elasticsearch, and under setup.kinbana. 
  ```bash
  output.elasticsearch:
  hosts: ["10.1.0.4:9200"]
  ```
  
  ```   
  setup.kibana:
  host: "10.1.0.4:5601"
  ```
- Run the playbook, and navigate to the Metricbeat installation page on the ELK server GUI to check that the installation worked as expected by clicking the check data button

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
  - There are three playbooks, they are filebeat-playbook.yml, metricbeat-playbook.yml, and install-elk.yml. The playbooks should be located in /etc/ansible/roles
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
  - You would need to edit the ansible hosts file to include your host name and IP addresses. To change the host machine that the playbook will run on, you would need to specify the host name in the playbook file at the beginning with the hosts option.
- _Which URL do you navigate to in order to check that the ELK server is running?
  - http://[Your-Elk-Machine-VM]:5601/app/kibana


