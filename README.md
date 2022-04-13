## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.
Project/diagrams/ELK STACK PROJECT.jpg

![ELK STACK PROJECT](https://user-images.githubusercontent.com/95455459/163074684-1deabc03-aacd-42c9-bc11-dce6b44e1227.JPG)

These files have been tested and used to generate 3 instances of the "Damned Vulnerable Web Application" and a live ELK stack deployment on an Azure cloud Resource group (although it is not limited to that environment the provided documentation will be for setup with Azure). By setting up the network architecture in the Azure portal and running the provided playbooks over SSH you will create the above network. Alternatively, you can install portions of the network on your own system using the provided playbooks individually (assuming your network has a similar topology and you are using ansible). This would allow you to install just the dvwa, or the elk stack and its beats depending on your preference.

dvwa playbook: Ansible/dvwaPlaybooks_Ansible/pentest.yml
elk playbook: Ansible/elkPlaybooks_Ansible/install-elk.yml
filebeat: Ansible/elkPlaybooks_Ansible/filebeat-playbook.yml
metricbeat: Ansible/elkPlaybooks_Ansible/metricbeat-playbook.yml

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

| Name     | Function | IP Address  | Operating System |
|----------|----------|-------------|------------------|
| Jump Box | Gateway  | 10.1.0.4    | Ubuntu/Linux     |
| Web-1    | Server   | 10.1.0.5    | Ubuntu/Linux     |
| Web-2    | Server   | 10.1.0.6    | Ubuntu/Linux     |
| Web-3    | Server   | 10.1.0.7    | Ubuntu/Linux     |
| ELK-VM   | Monitor  | 10.0.0.4    | Ubuntu/Linux     |
| LB       | Balancer |20.213.81.182| N/A              |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
(Home Workstation IP address)
(someone you trust's IP address)

Machines within the network can only be accessed by JumpBox, RedTeamVM. The JumpBox is also able to ssh to ElkVM on the adjacent ElkNet network thanks to adding the network peering. This is treated as internal traffic allowed by our secuirty rules, so the jumpbox is still the only way to ssh to the network.

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| RedTeamVM| Yes                 | #mypublicIP port 22  |
| Web-1    | No                  | RedTeamNet, ElkNet   |
| Web-2    | No                  | RedTeamNet, ElkNet   |
| Web-2    | No                  | RedTeamNet, ElkNet   |
| Elk VM   | No                  |ElkNet, RedteamNet, # mypublicIP port 5601|
| LB       | No                  | #mypublicIP port 80  |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it can save a lot of time and resources to automate the configurationg and deployment of webservers. Furthermore, running ansible playbooks is non-destructive, which makes the process much easier to troubleshoot as it reduces the amount of backtracking.

**The pentest.yml playbook implements the following tasks:**
turns off apache 2 (if present)
installs docker, python 3, and python docker module if not already present
downloads and launches dvwa in a docker web container running on port 80, ensures it will relaunch on reboot
enables docker service on boot

**The install-elk.yml playbook implements the following tasks:**
Installs docker, python 3, and pythong docker module if not already present
configures the system to use more memory (vm.max_map_count)
downloads and launches the elk container from docker, reserves ports 5601, 9200, and 5044
enables docker service on reboot

**The filebeat-playbook.yml implements the following tasks:**
downloads and installs the debian version of filebeat
drops in the config file in right directory
enables, sets up, and starts filebeat service
enables filebeat service on boot

**The metricbeat-playbook.yml implements the following tasks:**
downloads and installs the debian version of metricbeat
drops the metricbeat config file in right directory
enables, sets up, and starts metricbeat service
enables metricbeat service on boot

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

![dockerps2](https://user-images.githubusercontent.com/95455459/163076230-48bd0267-a10c-4ed0-affa-c17085b5492b.JPG)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- Web-1 10.1.0.5
- Web-2 10.1.0.6
- Web-2 10.1.0.7

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
Filebeat: When you start this beat, it starts one or more inputs that look in the locations you've specified for log data. For each log it finds it starts a harverster process to read for new content and sends that information to ELK -Metricbeat: Metricbeat helps you monitor your server by collecting metrics form the system and other services running on the server. It takes the metrics nad statistics it collects and ships them to ELK

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

ssh to the jumpbox from your public IP
start docker
sudo docker start <docker_name>

attach docker
sudo docker attach <docker_name>

Once you are SSH into ansible's docker container you can follow the steps below:

!Important: for elk to run, your VM will have to have at least 4GB memory, and 3.5 GB available

Copy the playbooks, and config files to /etc/ansible.

Update the hosts file in /etc/ansible to include webservers group

	# /etc/ansible/hosts
	[webservers]
	10.1.0.5 ansible_python_interpreter=/usr/bin/python3
	10.1.0.6 ansible_python_interpreter=/usr/bin/python3
	10.1.0.7 ansible_python_interpreter=/usr/bin/python3

	[elk]
	10.0.0.4 ansible_python_interpreter=/usr/bin/python3
	```
if neccesary, update the remote_login line in ansible.cfg

remote_user = sysadmin
tests if ansible is communicating with webservers

ansible all -m ping
if neccesary, add the links to the ssh keys in your host file

Update the filebeat-config file to include...

setup.kibana:
  host: "<ELK VM PRIVATE IP>:5601"
output.elasticsearch:
# Array of hosts to connect to.
hosts: ["<ELK VM PRIVATE IP>:9200"]
username: "elastic"
password: "changeme"

# Optional protocol and basic auth credentials.
#protocol: "https"
#username: "elastic"
#password: "changeme"
update the metricbeat-config file to include...

Under Kibana

setup.kibana:
host: "10.0.0.4:5601"
Under Elasticsearch Output

output.elasticsearch:
# Array of hosts to connect to.
hosts: ["10.0.0.4:9200"]
username: "elastic"
password: "changeme"

# Optional protocol and basic auth credentials.
#protocol: "https"
#username: "elastic"
#password: "changeme"
Run the desired playbook(s), and navigate to http://{ElkVM Public IP}:5601/app/kibana#/home to check that the installation worked as expected. Wait till you have verified Kibana is running before you update the login credentials, then update the config file.

![Kibana](https://user-images.githubusercontent.com/95455459/163077082-4a755839-4fc4-48ee-aa4e-e79aff9af6fb.JPG)

click on explore your own and you will be redirected to the main Kibana page, where you can then access the logs from the beats If you want to download these files, feel free to clone the repository with

git clone https://github.com/vdudar/Project.git
