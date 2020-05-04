## Automated ELK Stack Deployment
The files in this repository were used to configure the network depicted below. 
(Images/Network_Diagram.png)

 These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat-playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

elk.yml -pentest.yml -filebeat-configuration.yml -filebeat-playbook.yml -metricbeat-configuration.yml -metricbeat.yml
This document contains the following details:

### Description of the Topologu
Access Policies
ELK Configuration
Beats in Use
Machines Being Monitored
How to Use the Ansible Build
Description of the Topology
The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly _redundant, in addition to restricting access to the network.

What aspect of security do load balancers protect? What is the advantage of a jump box? 
Load balancer protect the user authintication, mitigate DoS attacks, and provides a website an external IP address that is accessed by the internet. 
Advantage of jump box is running an Ansible container, which has a full access to the VNet and used that container to configure another VM running a DVWA container.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the metrics and system files_.

What does Filebeat watch for?_ 
Filebeat collects data about the file system, (monitors the log files, and forwards them to Elasticsearch or Logstash.).
What does Metricbeat record?_ 
Metricbeat collects machine metrics, such as uptime. 
The configuration details of each machine may be found below.


| Name     | Function| IP Address|  Operating System  |  
|----------|---------|-----------|--------------------|
|Jump Box 	|Gateway  |10.0.0.4	  |Linux (Ubuntu 18.04)|    
|DVWA-VM1	 |host DVWA|10.0.0.8	  |Linux (Ubuntu 18.04)|
|DVWA-VM2  |host DVWA|10.0.0.9   |Linux (Ubuntu 18.04)|
|ELK-Server|host ELK	|10.0.0.14  |Linux (Ubuntu 18.04)| 


### Access Policies
The machines on the internal network are not exposed to the public Internet.

Only the Load Balancer machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

XX.XX.0.0/16 ( home machine IP address)
Machines within the network can only be accessed by Jump box via SSH_.

Which machine did you allow to access your ELK VM? What was its IP address?_
Jump box , 40.83.146.75
A summary of the access policies in place can be found in the table below.

Name	Publicly Accessible	Allowed IP Addresses
Jump Box	Yes	10.0.0.4
DVWA-VM1	No	X.X.0.0/16
DVWA-VM2	No	X.X.0.0/16
ELK-Server	No	X.X.0.0/16
| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box |	Yes	                | 10.0.0.4             |
|DVWA-VM1	 | No	                 |   X.X.0.0/16         |
|DVWA-VM2	 | No                  |  	X.X.0.0/16         |
|ELK-Server| No                  |  	X.X.0.0/16         |

### Elk Configuration
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it guarantees that configuration is identicall.
-What is the main advantage of automating configuration with Ansible?_ 
Configuration with Ansible is agentless, where is done by running the playbook files to automate one device or multiple devices.

The playbook implements the following tasks: 
-explain the steps of the ELK installation play.

Install Docker: The playbook should install the docker.io, python-pip, and docker, which is the Docker Python module services
name: Install docker.io 
apt:
force_apt_get: yes 
name: docker.io 
state: present

Use apt module
name: Install pip
apt:
force_apt_get: yes 
name: python-pip
state: present

Use the Ansible pip module to install docker
name: Install Docker python module
pip:
name: docker
state: present

this task of the playbook is to increase the virtual memory on the VM, which is a system requirement for the ELK container
name: Increase virtual memory 
command: sysctl -w vm.max_map_count=262144

Use docker_container module, Launching and Exposing the Container, so after Docker is installed, download and run the image sebp/elk container that should be started with the following published ports.
name: download and launch a docker elk container
docker_container:
name: elk
image: sebp/elk
state: started 
published_ports:
- 5601:5601 
- 9200:9200 
- 5044:5044

The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.

(Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
-10.0.0.8 
-10.0.0.9

We have installed the following Beats on these machines:
metricbeat
filebeat
These Beats allow us to collect the following information from each machine:
#Explain what kind of data each beat collects, and provide 1 example of what you expect to see.

Filebeat: 
Filebeat is used to collect log files from very specific files, such as those generated by Apache, Microsoft Azure tools, the Nginx web server, and MySQL databases.
( Specifically, it logs information about the file system, including which files have changed and when).
Metricbeat: collects machine metrics, such as uptime.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:

Copy the filebeat-configuration file to the Ansible container.
Update the filebeat-configuration file to include the ELK server's IP address, because we are connecting the DVWA machines to the ELK server.
Run the playbook, and navigate to (VM_ELK serve'r IP address:5601) Kibana_ to check that the installation worked as expected.
As a Bonus, provide the specific commands the user will need to run to download the playbook, update the files, etc.
