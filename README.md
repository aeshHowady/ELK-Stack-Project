## Automated ELK Stack Deployment
The files in this repository were used to configure the network depicted below.⋅⋅ 
...(Images/Network_Diagram.png)

 These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat-playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

- elk.yml 
- pentest.yml 
- filebeat
- configuration.yml
- filebeat-playbook.yml
- metricbeat-configuration.yml 
- metricbeat.yml
This document contains the following details:

### Description of the Topologu
- Access Policies
- ELK Configuration
- Beats in Use
- Machines Being Monitored
- How to Use the Ansible Build
- Description of the Topology

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

Note: Due to the limitation of the Azure supsicription, I was not able to create the whole number of VM as suppose to be, and as it is shown in the Network diagram.


### Access Policies
The machines on the internal network are not exposed to the public Internet.

Only the Load Balancer machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:

XX.XX.0.0/16 ( home machine IP address)
Machines within the network can only be accessed by Jump box via SSH_.

Which machine did you allow to access your ELK VM? What was its IP address?_
Jump box , 40.83.146.75
A summary of the access policies in place can be found in the table below.

Name	Publicly Accessible	Allowed IP Addresses

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box |	Yes	                |   10.0.0.4           |
|DVWA-VM1	 | No	                 |   X.X.0.0/16         |
|DVWA-VM2	 | No                  |  	X.X.0.0/16         |
|ELK-Server| No                  |  	X.X.0.0/16         |

### Elk Configuration
Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it guarantees that configuration is identicall.
-What is the main advantage of automating configuration with Ansible?_ 
Configuration with Ansible is agentless, where is done by running the playbook files to automate one device or multiple devices.

The playbook implements the following tasks: 
-explain the steps of the ELK installation play.

Install Docker: -_The playbook should install the docker.io, python-pip, and docker, which is the Docker Python module services. 
name: Install docker.io
...apt:⋅⋅
...force_apt_get: yes⋅⋅
...name: docker.io⋅⋅ 
...state: present⋅⋅

... name: Install pip⋅⋅
...apt:⋅⋅
...force_apt_get: yes⋅⋅ 
...name: python-pip⋅⋅
...state: present⋅⋅

- name: Install Docker python module
- pip:
- name: docker
- state: present

-_this task of the playbook is to increase the virtual memory on the VM, which is a system requirement for the ELK container
name: Increase virtual memory 
command: sysctl -w vm.max_map_count=262144

-_Use docker_container module, Launching and Exposing the Container, so after Docker is installed, download and run the image sebp/elk container that should be started with the following published ports.
- name: download and launch a docker elk container
- docker_container:
- name: elk
- image: sebp/elk
- state: started 
- published_ports:
- 5601:5601 
- 9200:9200 
- 5044:5044

The following screenshot displays the result of running docker ps after successfully configuring the ELK instance.

(Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.8 
- 10.0.0.9

We have installed the following Beats on these machines:
- metricbeat
- filebeat
These Beats allow us to collect the following information from each machine:
#Explain what kind of data each beat collects, and provide 1 example of what you expect to see.

Filebeat: 
- Filebeat is used to collect log files from very specific files, such as those generated by Apache, Microsoft Azure tools, the Nginx web server, and MySQL databases.
( Specifically, it logs information about the file system, including which files have changed and when).
- Metricbeat: collects machine metrics, such as uptime.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned:

SSH into the control node and follow the steps below:

Copy the filebeat-configuration file to the Ansible container.
Update the filebeat-configuration file to include the ELK server's IP address, (10.0.0.14), because we are connecting the DVWA machines to the ELK server.
Run the playbook, and navigate to (VM_ELK serve'r IP address:5601) Kibana_ to check that the installation worked as expected.
- Bonus:
#### Ansible Playbooks
Connect to the jump box, and connect to the Ansible container in the box.
- run docker container list -a.
- docker start [container_name]
- docker attach [container_name]
- Create a YAML playbook file that will be used for the configuration.
- nano /etc/ansible/pentest.yml
 Then, run this Ansible playbook on the new virtual machine.
- run: ansible-playbook pentest.yml
Then to verify that DVWA is running on the new VM, SSH to the new VM from your Ansible container.
- run: curl localhost/setup.php 
If everything is working, you should get back some HTML from the DVWA container.

#### ELK Installation
- Step_1  Creating a New VM to run ELK
- Step_2 Downloading and Configuring the Container
- Add the new VM to the Ansible hosts file.
- Create a new Ansible playbook to use for thte new ELK virtual machine.
- name: Config elk VM with Docker
  hosts: elkservers
  remote_user: elk
  become: true
  tasks:
- Step_3 Launching and Exposing the Container
- After Docker is installed, download and run the sebp/elk container.
The container should be started with these published ports:
5601:5601
9200:9200
5044:5044
- SSH from Ansible container to your ELK machine to verify the connection before you run playbook.
- ssh username@ELK_IP_address 
then exit, and run:
ansible-playbook elk.yml  
- Step_4 Identity and Access Management
This done in Azure lab
ELK web server runs on port 5601. Create an incoming rule for the security group that allows TCP traffic over port 5601 from your IP address.
Verify that you can load the ELK stack server from the browser at http://[ELK.VM.IP]:5601.
You should see Kibana website as in (Images/Kibana1.png).
#### Filebeat Installation
- Installing Filebeat on the DVWA Container
- Navigate to http://[ELK.VM.IP]:5601. 
- If it is not running, open a terminal on your computer and SSH into the ELK server.Then, run:  docker container list -a 
to verify that the container is on.
If it isn't, run:  docker start sebp/elk.
- Open the ELK server homepage.
- Click on Add Log Data.
- Choose System Logs.
- Click on the DEB tab under Getting Started to view the correct Linux Filebeat installation instructions.
#### Metricbeat Installation 
- Creating a Play to Install Metricbeat
- To update your Ansible playbook to install Metricbeat:
From the homepage of your ELK site:
- Click Add Metric Data.
- Click Docker Metrics.
- Click the DEB tab under Getting Started for the correct Linux instructions.
- Update your playbook with tasks that perform the following:
- Download the Metricbeat .deb file.
+ Use dpkg to install the .deb file.
+ Update and copy the provided Metricbeat config file.
+ Run the ./metricbeat modules enable docker command.
+ Run the ./metricbeat setup command.
+ Run the ./metricbeat -e command.

