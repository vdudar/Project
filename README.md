## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.
Project/diagrams/ELK-STACK-PROJECT.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

  - install-elk.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly secured, in addition to restricting access to the network.
- The off-loading function of a load balancer defends an organization against distributed denial-of-service (DDoS) attacks. It does this by shifting attack traffic from the corporate server to a public cloud provider.
jump box is a system on a network that accesses and manages all the devices in a different zone of security. It is a hardened device that spans two different security zones and enables a controlled means of access between them. Admins use the hardened server to “jump” through to access other servers

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system configuration.
Filebeat is a lightweight shipper for forwarding and centralizing log data.
Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash. 

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.1.0.4   | Linux            |
| Web-1    | VM       | 10.1.0.5   | Linux            |
| Web-2    | VM       | 10.1.0.6   | Linux            |
| ELK-Ser  | ELK      | 10.0.0.4   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
My IP Adress

Machines within the network can only be accessed by JumpBox.

From JumpBox to Ansible container and than to ELK, 

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | My IP Address        |
|          |                     |                      |
|          |                     |                      |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
Ansible automation helps considerably with the representation of Infrastructure as Code (IAC). IAC involves provisioning and management of computing infrastructure and related configuration through machine-processable definition files.

The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- Configure Elk VM with Docker
- Install docker.io
- Install pip3
- Install Docker python module
- Use more memory
- download and launch a docker elk container
- Enable service docker on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

Project/images/dockerps

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 10.1.0.5
- Web-2 10.1.0.6

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
- The Filebeat Elasticsearch module can handle audit logs, deprecation logs, gc logs, server logs, and slow logs.
- Metricbeat is a lightweight shipper that you can install on your servers to periodically collect metrics from the operating system and from services running on the server. Metricbeat takes the metrics and statistics that it collects and ships them to the output that you specify, such as Elasticsearch or Logstash.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the playbook file to /etc/ansible.
- Update the nano file to include...
- Run the playbook, and navigate to /etc/ansible to check that the installation worked as expected.

_TODO: Answer the following questions to fill in the blanks:_
- .yml file with the playbook contant, should be coppied to the /etc/ansible
- this information should be included in the config file, hosts, and the actual playbook

- _Which URL do you navigate to in order to check that the ELK server is running?
- Navigate to Azure than virtual machines and check status https://portal.azure.com/#blade/HubsExtension/BrowseResource/resourceType/Microsoft.Compute%2FVirtualMachines

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
curl https://github.com/vdudar/Project/blob/main/ansible/install-elk.yml
	to download the playbook
nano install-elk.yml 
	to edit the playbook
