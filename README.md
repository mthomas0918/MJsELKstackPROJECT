## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/ELKstackNETdiagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.
  - _TODO: Enter the playbook file.
---
   - name: Installing and Launch Filebeat
  hosts: webservers
  become: yes
  tasks:

  - name: Download filebeat .deb file
    command: curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb

  - name: Install filebeat .deb
    command: dpkg -i filebeat-7.4.0-amd64.deb

  - name: Drop in filebeat.yml
    copy:
      src: /etc/ansible/files/filebeat-config.yml
      dest: /etc/filebeat/filebeat.yml

  - name: Enable and Configure System Module
    command: filebeat modules enable system


  - name: Setup filebeat
    command: filebeat setup

  - name: Start filebeat service
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
- _TODO: What aspect of security do load balancers protect? Load balancers protect organizations from DDoS attacks by shifting attack traffic from the server to the public cloud. What is the advantage of a jump box? Using a jump box provides a low risk of threats and more security before launching any tasks to the server(s).

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the server and system network.
- _TODO: What does Filebeat watch for? Filebeat monitors the log files/locations, collects log events, and forwards them to get indexed and archived.
- _TODO: What does Metricbeat record? Metricbeat collects metrics and statistics from OS and services running from the server; which in turn gets indexed and archived.


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

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: Add whitelisted IP addresses (10.0.0.5, 10.0.0.6)

Machines within the network can only be accessed by SSH.
- _TODO: Which machine did you allow to access your ELK VM? Jump Box What was its IP address? 10.1.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box |       Yes/No        | 10.0.0.5 10.0.0.6    |
|          |                     |                      |
|          |                     |                      |




### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible? The main advantage is that instead of installing things one-by-one, using Ansible and creating a playbook could make the process much easier and install multiple things only using one command.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
•	Install docker.io (command: apt) – after installation, you would make it present so that it can be used. It’s the same with installing Python3-pip3 (command: apt) and the Docker Python Module (command: PIP).
•	Increase virtual memory and allow more memory to be used. The command used within the playbook to execute is Sysctl. Numerical settings are to be set at the max “262144” and make the state present.
•	Download and launch docker containers for ELK. Obtain images, set restart policy to always, and set the stated to be started. Allow published ports: 5601, 9200, and 5044 so that ELK can be accessed. 


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/ELK-Installation.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring: 10.0.0.4, 10.0.0.5, 10.0.0.6 10.1.0.4

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed: Filebeat & Metricbeat

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc.
•	Filebeat – monitors, collects, and forwards centralized logs and files.
•	Metricbeat – records, collects, and forwards metrics from system and service statistics.




### Using the Playbook

In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-7.4.0-amd64.deb file to filebeat-config.yml
- Update the filebeat-config.yml file to include...
	
     
      output.elasticsearch:
	Hosts: [“10.1.0.4:9200”]
	Username: “elastic”
	Password: “changeme”
	
       …

      setup.kibana:
	host: “10.1.0.4:5601”


- Run the playbook, and navigate to Kibana to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? filebeat.yml Where do you copy it? /etc/filebeat/filebeat.yml
- _Which file do you update to make Ansible run the playbook on a specific machine? filebeat-config.yml How do I specify which machine to install the ELK server on versus which to install Filebeat on? In the playbook file, “host” differentiates how and where to install.
- _Which URL do you navigate to in order to check that the ELK server is running? http://[ELKvmPUBLICip]:5601/app/kibana

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc.
•	curl -L -O https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.4.0-amd64.deb
•	dpkg -i filebeat-7.4.0-amd64.deb
•	Filebeat modules enable system
•	Filebeat setup
•	Filebeat start
