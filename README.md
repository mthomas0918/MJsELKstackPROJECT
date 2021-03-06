## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

(Images/ELKstackNETdiagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

---
   - Installing and Launch Filebeat
  hosts: webservers
  become: yes
  tasks:

  - Download filebeat .deb file
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

  - Install filebeat .deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

  - Drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

  - Enable and Configure System Module
    command: filebeat modules enable system


  - Setup filebeat
    command: filebeat setup

  - Start filebeat service
    command: service filebeat start

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build

### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly responsive, in addition to restricting multiple requests to the network.
Load balancers protect organizations from DDoS attacks by shifting attack traffic from the server to the public cloud. Using a jump box provides a low risk of threats and more security before launching any tasks to the server(s).

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the server and system network.
Filebeat monitors the log files/locations, collects log events, and forwards them to get indexed and archived.
Metricbeat collects metrics and statistics from OS and services running from the server; which in turn gets indexed and archived.


The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.4   | Linux            |
| Web-1    | Server   | 10.0.0.5   | Linux            |
| Web-2    | Server   | 10.0.0.6   | Linux            |
| ELK      | Gateway  | 10.1.0.4   | Linux            |

### Access Policies
The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: (10.0.0.5, 10.0.0.6)

Machines within the network can only be accessed by SSH.
Jump Box 10.1.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box |       Yes/No        | 10.0.0.5 10.0.0.6    |
|          |                     |                      |
|          |                     |                      |




### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
Instead of installing things one-by-one, using Ansible and creating a playbook could make the process much easier and install multiple things only using one command.

The playbook implements the following tasks:
-	Install docker.io (command: apt). After installation, you will make it present so that it can be used. It is the same with installing Python3-pip3 (command: apt) and the Docker Python Module (command: PIP).
-	Increase virtual memory and allow more memory to be used. The command used within the playbook to execute is Sysctl. Numerical settings are to be set at the max 262144 and make the state present.
-	Download and launch docker containers for ELK. Obtain images, set restart policy to always, and set the state to be started. Allow published ports: 5601, 9200, and 5044 so that ELK can be accessed. 


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

(Images/ELK-Installation.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
10.0.0.5, 10.0.0.6

We have installed the following Beats on these machines:
Filebeat & Metricbeat

These Beats allow us to collect the following information from each machine:
-	Filebeat monitors, collects, and forwards centralized logs and files.
-	Metricbeat records, collects, and forwards metrics from system and service statistics.




### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-7.4.0-amd64.deb file to filebeat-config.yml
- Update the filebeat-config.yml file to include...
	
     
      output.elasticsearch:
	Hosts: 10.1.0.4:9200
	Username: elastic
	Password: changeme
	

      setup.kibana:
	host: 10.1.0.4:5601


Run the playbook, and navigate to Kibana to check that the installation worked as expected.

-	filebeat.yml > /etc/filebeat/filebeat.yml

Update filebeat-config.yml to make Ansible run the playbook on a specific machine. 
- In the playbook file, host differentiates how and where to install. It will help specify where the installation occurs.
- http://[ELKvmPUBLICip]:5601/app/kibana is the URL used to navigate to in order to check that the ELK server is running.

These are the specific commands needed to run to download the playbook, update the files, etc.
-	curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb
-	dpkg -i filebeat-7.4.0-amd64.deb
-	Filebeat modules enable system
-	Filebeat setup
-	Filebeat start
