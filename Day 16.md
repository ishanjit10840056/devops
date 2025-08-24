Day 16



###### ANSIBLE: Automation with Ansible

------------------------------------------



Automation tools:

&nbsp;	Puppet

&nbsp;	Chef

&nbsp;	Salt stack

&nbsp;	Ansible



For puppet chef and salt stack this is a slab between controller and server, that helps in

maintaining code and updates. It's slower and high cost



Ansible do not have any slab between controller machine and remote server

Ansible is developed in python and uses push mechanism, 

it is developed by Michael DeHaan.



Ansible converts command automatically in python.

Python should be installed on the machines ansible is working on

LINUX has one process to communicate i.e., SSH authentication

SSH uses **SCP protocol** in backend to communicate, it helps in carrying the code

from controller machine to remote server



---------------------------------------------------------------------------------------------

Host 3 instances -> one controller two node(server)

set DNS -> vim /etc/hosts

go to

vim /etc/ssh/sshd\_config -> 40, 65 yes -> in all 3

systemctl restart sshd

systemctl enable sshd



GENERATE KEY IN CONTROLLER

ssh-keygen

cd. ssh/



set password of root in both the nodes

ssh-copy-id root@privateipofnode  -> to be done in controller machine

or go and paste controller's id\_rsa.pub to node cd .ssh/ cat authorized\_keys









scp /etc/hosts root@privateipofnode:/etc/hosts



----------------------------------------------------------------------------------------------



AAP -> Ansible automation platform



IN CONTROLLER



yum update -y

yum install pip

pip/yum install ansible-core\*



ansible --version



cd /etc/

mkdir ansible if not present

or else go to cd /etc/ansible



Go to sir's repo -> https://github.com/sanjayguruji/ansible-code

copy content of ansible.cfg

and create a vim ansible.cfg and paste the content



ansible --version





make group wise implementation



vim hosts



\[dev-server]

node-one



\[prod-server]

node-two



























