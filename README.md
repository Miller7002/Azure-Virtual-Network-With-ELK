## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[Network Diagram](https://github.com/Miller7002/Azure-Virtual-Network-With-ELK/blob/8f10c9273e27135955cc6251973ebd3d5a7f4e67/Diagrams/Virtual_Network.png)

![image](https://user-images.githubusercontent.com/88359985/147163618-a16f0a1f-c18a-4904-8adc-f198bfb15c4a.png)


These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

[Filebeat.yml](https://github.com/Miller7002/Azure-Virtual-Network-With-ELK/blob/main/Ansible/Filebeat.yml)

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting access to the network.
- A load balancer serves many purposes including protecting from attacks such as DDoS and also making sure traffic is routed to a running Virtual Machine if in the event one is down. 
- The advantage of having a Jump Box is to restrict traffic from the internet to the Virtual Machines on the network.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the log files and system metrics.
- Filebeat records and monitors logs files from Web 1 and Web 2
- Metricbeat records metrics from Web 1 and Web 2

The configuration details of each machine may be found below.

| Name    | Function        | IP Address             | Operating System |
|---------|-----------------|------------------------|------------------|
| Jumpbox | Gateway         | 20.124.34.184          | Linux            |
| Web 1   | Virtual Machine | 10.1.0.21              | Linux            |
| Web 2   | Virtual Machine | 10.1.0.20              | Linux            |
| Elk     | ELKStack        | 40.122.162.46/10.2.0.4 | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- My personal host machine's public IP.

Machines within the network can only be accessed by SSH via the Jump Box.
- 20.124.34.184

A summary of the access policies in place can be found in the table below.

| Name    | Publicly Accessible | Allowed IP Addresses                 |
|---------|---------------------|--------------------------------------|
| Jumpbox | NO                  | Host Machine Public IP               |
| Web 1   | NO                  | 20.124.24.184                        |
| Web 2   | NO                  | 20.124.24.184                        |
| ELK     | NO                  | Host Machine Public IP/20.124.24.184 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- It allows you to install multiple things using a playbook at once on many Virtual Machines all at the same time. With Ansible you save a lot of time and there is no need to go into each Virtual Machine and install something 1 by 1.

The playbook implements the following tasks:
- Installs docker.io
- Installs python3-pip
- Installs docker module
- Increase virtual memory
- Download and install docker elk container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

[docker ps Screenshot](https://github.com/Miller7002/Azure-Virtual-Network-With-ELK/blob/main/Ansible/docker%20ps%20Screenshot/Screenshot%20(69).png)
![image](https://user-images.githubusercontent.com/88359985/147294990-7f6da034-4674-4e7f-9576-dc9baf0d8643.png)


### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web 1: 10.1.0.21
- Web 2: 10.1.0.20

We have installed the following Beats on these machines:
- Filebeat
-  Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat will monitor log files and log events and send a copy of them to either Elasticsearch or Logstash for indexing. You can use these logs to track what a user is doing on either Virtual Machine.
- Metricbeat will collect metrics and data and send it to either Elasticsearch or Logstash. You can expect to see metrics and data from the operating systems and services running on the Virtual Machines

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to /etc/ansible within the ansible container.
- Update the hosts file to include the IP addresses of what Virtual Machines you want to run the playbook on.
- Run the playbook, and navigate to Kibana via your web browser (http//:[VM public IP]:5601/app/kibana) and check to make sure Kibana is receiving log data from the VM's you ran the playbook on.

- _Which file is the playbook? Where do you copy it?_
  - The file for the playbook would be the following YAML file [ELK.yml](https://github.com/Miller7002/Azure-Virtual-Network-With-ELK/blob/main/Ansible/ELK.yml) and you would place it in /etc/ansible within the anisble container on your host machine.
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
  - You would update the hosts file within the ansible container at the /etc/ansible directory to include the private IP addresses of the Virtual Machines you wish to run the playbook against whether it be for Filebeat or the ELK server. If you are only wanting to run a playbook against specific Virtual Machines, you can add additional 'webservers' sections to include the specific private IP addresses. Once you have specified the IP addresses in the 'hosts' file, you will then need to change which host you want the playbook to run against in the playbook YAML file.
- _Which URL do you navigate to in order to check that the ELK server is running?
  - http//:[VM public IP]:5601/app/kibana
