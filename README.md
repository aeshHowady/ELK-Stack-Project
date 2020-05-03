
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.
(Images/Network_Diagram.png)

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the filebeat-playbook.yml file may be used to install only certain pieces of it, such as Filebeat.

  - elk.yml
  -pentest.yml
  -filebeat-configuration.yml
  -filebeat-playbook.yml
  -metricbeat-configuration.yml
  -metricbeat.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly __redundant_, in addition to restricting __access__ to the network.
- _TODO: What aspect of security do load balancers protect? What is the advantage of a jump box?
Loadbalancer protect the availability aspect of security, such as to mitigate DoS attacks.Also, a load balancer provides a website an external IP address that is accessed by the internet.
Advantage of jump box is running an Ansible container, which has a full access to the VNet and used that container to configure another VM running a DVWA container.




Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the __metrics__ and system __files___.
- What does Filebeat watch for?_
Filebeat collects data about the file system, (monitors the log files, and forwards them to Elasticsearch or Logstash.).
- What does Metricbeat record?_
Metricbeat collects machine metrics, such as uptime.
The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name         | Function        | IP Address | Operating System    |
|--------------|-----------------|------------|---------------------|
| Jump Box     | Gateway         | 10.0.0.4   | Linux (Ubuntu 18.04)|
| DVWA-VM1     |  host DVWA      | 10.0.0.8   |Linux (Ubuntu 18.04) |
| DVWA-VM2     |  host DVWA      | 10.0.0.9   |Linux (Ubuntu 18.04) |
|ELK-Server    |  host ELK       | 10.0.0.14  |Linux (Ubuntu 18.04) |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Load Balancer machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- XX.XX.0.0/16  ( home machine IP address) 

Machines within the network can only be accessed by _Jump box via SSH__.
- Which machine did you allow to access your ELK VM? What was its IP address?_
Jump box , 40.83.146.75 

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box |  Yes                | 10.0.0.4             |
|DVWA-VM1  |  No                 | X.X.0.0/16           |
|DVWA-VM2  |  No                 | X.X.0.0/16           |
|ELK-Server|  No                 | X.X.0.0/16           |


### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it garantees that configuration is identicall.
-What is the main advantage of automating configuration with Ansible?_
Configuration with Ansible is agentless, where is done by running the playbook files to automate one device or multiple devices.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- ...
- ...

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![TODO: Update the path with the name of your screenshot of docker ps output](Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_

We have installed the following Beats on these machines:
- _TODO: Specify which Beats you successfully installed_

These Beats allow us to collect the following information from each machine:
- _TODO: In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the _____ file to _____.
- Update the _____ file to include...
- Run the playbook, and navigate to ____ to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it?_
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?_
- _Which URL do you navigate to in order to check that the ELK server is running?

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
