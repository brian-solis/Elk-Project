## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

![TODO: Update the path with the name of your diagram](Images/diagram_filename.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

  Link File From Github
## filebeat-config.yml
## filebeat-playbook.yml
## metricbeat-configuration.yml
## metricbeat-playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network. Load balancers protect the application responsiveness and also increases availability of applications and websites 
A jumpbox manages different devices in a separate security zone. A jump server is a hardened and monitored device that spans two dissimilar security zones and provides a controlled means of access between them,

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the data and system logs.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name    | Function   | Ip Address     | Operating System |   |
|---------|------------|----------------|------------------|---|
| JumpBox | Gateway    | 52.188.11.211  | Ubuntu 18.04.6   |   |
| Elk-VM  | Monitoring | 104.42.102.109 | Unbuntu 18.04.6  |   |
| Web-1   | Web Server | 20.127.21.139  | Unbuntu 18.04.6  |   |
| Web-2   | Web Server | 20.127.21.139  | Unbuntu 18.04.6  |   |


### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the allowed machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses: 66.68.116.26 


Machines within the network can only be accessed by the jumphost Private IP 10.2.0.7 Public IP 52.188.11.211 and the elk-vm with the private ip of 10.2.0.4 and public ip of 104.42.102.109 

A summary of the access policies in place can be found in the table below.
| Name     | Publicly Accessible | Allowed IP address |   |   |
|----------|---------------------|--------------------|---|---|
| Jump-Box | Yes                 | SSH from Whitelist IP       |   |  
| Elk-VM      | NO                 | SSH from Whitelist IP       |   |   |
| Web-1       | NO                 |
| Web-2       | NO                 |                    


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
There are no agents or software deployed on the clients/servers to work with Ansible. The connection can be done through the SSH or using the Python.

The playbook implements the following tasks:
- Intalls the following modules within the container: docker.io, pip3, Install Docker python module 
- Configures sysctl modules 
- Sets the docker services to boot on docker start


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.
### Target Machines & Beats
This ELK server is configured to monitor the following machines:
-Web-1: 10.1.0.10
-Web-2: 10.1.0.5


We have installed the following Beats on these machines:
- Filebeats version 7.6.2
- Metricbeats version 7.6.2


These Beats allow us to collect the following information from each machine:
- Filebeats - a lightweight way to forward and centralize logs and files
- Metricbeats - a lightweight way to send system and service statistics


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat.configuration & metricbeat.config file to the ansible container.
- Update the configuration files to include correct IP addresses of the local IP addresses for the Elk Server @ 10.2.04:5601 and 10.2.0.4:9200.
- Run the filebeat and metricbeat playbooks, and navigate to the proper URL of the ELK server GUI (Kibana http://<public ELK IP address>:5601/app/kibana#/home) to check that the installation worked as expected.

The two playbook files you will need to be concerned with is filebeat-playbook.yml & metricbeat-playbook.yml, both of these will need to be copied to the /etc/ansible/roles directory on your ansible container on the jump box provisioner.

You will need to update filebeat-config.yml & metricbeat-configuration.yml specifically on lines 1106 and 1806 and add the ELK private IP address of 10.2.0.4 so that the deployment of the Kibana monitor modules are deployed properly on the ELK server.

## Line 1106 - update the hosts IP address to your ELK servers private IP
hosts: ["10.2.0.4:9200"]
username: "elastic"
password: "changeme" # TODO: Change this to the password you set

## Line 1806 - perform the same action as above
setup.kibana:
  host: "10.2.0.4:5601" # TODO: Change this to the IP address of your ELK server

You will also need to update the hosts file in the ansible container to show proper targets for deployment and monitoring for the ELK server 

[webservers]
10.1.0.10 ansible_python_interpreter=/usr/bin/python3
10.1.0.5 ansible_python_interpreter=/usr/bin/python3

[elk]
10.2.0.4 ansible_python_interpreter=/usr/bin/python3

Once all of the configurations have been set run the follwing commands from the ansible container:

ansible-playbook filebeat-playbook.yml
ansible-playbookmetricbeat-playbook.yml

If the deployment has run sucessfully you should be able to head to http://<public ELK IP address>:5601/app/kibana#/home and Kibana should come up in your web browser.
