Web server make it live attach 5gb ebs vol 10 files


•	Create an instaces with sec grp http port 80
•	yum install httpd -y
•	rmpquery httpd
•	cd /var/www/html
•	cat > index.html
•	cd
•	systemctl start httpd
•	systemctl enable httpd
•	allow port 80 in inbound rules -> http
•	copy public ip and paste ip:80
•	volumes - name it to vol1
•	then create new volume (make sure to create in same availability zone)
•	Select GP2 - size 5gb
•	Now go to EBS - volumes
•	select the new generate volume - action -> attach
•	select instance hosted name
•	attach
•	Go to console
•	lsblk -fs
•	blkid
•	file system command
•	mkfs.ext4 /dev/xvdb --> formats the new disk - a for disk 1 b for disk 2 ..
•	blkid
•	create a mount point
•	mkdir /data
•	mount /dev/xvdb /data/
•	df -h - to check if storage is mounted
•	cd /data/
•	touch Sanjaya.txt{1..100} -> to create 100 files
•	THIS WAS TEMPORARY MOUNTING
•	TO MAKE IT PERMANENT-.
•	cd
•	blkid -> copy the uuid of partition b
•	open
•	vim /etc/fstab
•	paste the uuid - to mount it permanenetly or by name - /dev/xvdb /data ext4 defaults 0 0
•	it means mount xvdb to data folder using ext4 file system
•	RESTART THE INSTANCE
•	Check it another region

#########################################################################

One team working in one region imp data in ebs another team another region want to access that data(snapshot)

create a volume
then create snapshot of the volume
then go to snapshot - create volume from snapshot
and select new availability zone
open another terminal in that region to check the data copied
•	select the volume from ebs
•	actions
•	create snapshot
•	give description & click on create snapshot
•	Now go to Snapshots -> wait for status to be completed
•	Now select the snapshot -> actions -> copy snapshot -> choose destination - us-east-2
•	and click on copy snapshot
•	Change the region to Ohio & check the snapshot
•	Now in Ohio region
•	Create a new instance and launch it as we do with us-east-2a subnet
•	Connect to the instance in terminal
•	sudo su -
•	lsblk -> to check for the disk
•	Now go to snapshot -> select it -> actions -> create volume from snapshot
•	Select same zone and create
•	Now select it from volume - attach it with the instance
•	Now lsblk
•	blkid to check for file system
•	file system is also copied
•	mkdir /test
•	mount /dev/xvdb /test/
•	cd /test
•	ll
•	all data will be present

#################################################################

config web server in Mumbai region need same server into Singapore ami

•	set hostname
•	yum install httpd -y
•	rmpquery httpd
•	cd /var/www/html
•	cat > index.html
•	cd
•	systemctl start httpd
•	systemctl enable httpd
•	allow port 80 in inbound rules -> http
copy public ip and paste ip:80
yum install vsftpd -y
yum install cifs-utils -y
yum install nfs-utils -y
yum install tree -y
cat > file.txt
useradd jk
passwd jk
ROOT volume is the drive with Operating system
we need to do certain steps everyday so to get over it we create custom AMI
CUSTOM AMI SETUP
•	go to AMI's catalog to see multiple ami's
•	go to the instance hosted (shut-down and then do) -
•	select the instance
•	click on actions
•	image and templates
•	create a image
•	go to create image
•	Keep the reboot instance to unticked
•	delete on termination - untick
•	then click on create AMI
•	Now go to AMI and you will see your AMI created
•	You can terminate the old instance ( if delete on termination was unticked)
•	create new instance - select my ami and you will see your ami
•	select it and go ahead
•	copy the new ip and paste it on brower ip:80 and you will see the existing file that you created
•	you can also check on console or terminal for the files, users, services installed - all will be present
To share your AMI with other -
select the AMI
Actions
Edit permission
Add Shared accounts -> add account id
Now save changes
The other person can see you AMI in private ami's
Q1. How can you migrate a server from one region to another region
•	Go to images - AMIs
•	select the AMI
•	Click on actions -> copy AMI
•	Choose new destination region ex. Ohio
•	click on copy AMI
•	and switch to Ohio region from the main menu drop down
•	Check the AMI image -> and check on status until it becomes available
•	AMI is a region based service.
Again go to AMI - select the AMI scroll down and click on permissions
Then edit AMI permission
Add account id and paste the AWS account id of the one who you want to share
Then apply
search on private ami and you will find it
Then deploy an instance using that AMI
If you want to delete - deregister the AMI and delete the snapshot


#########################################################################

Efs common project in same region share project

•	EFS in aws is replica of NFS
•	Deploy ec2 instance in same region different zone
•	machine 1 : Amazon Linux, us-east-1a, new security group -> LAUNCH
•	machine 2 : Ubuntu, us-east-1b, existing security group -> LAUNCH
•	machine 3 : RedHat, us-east 1c ..
•	Search for EFS -> create file system -> set name(my-efs) -> VPC default -> create file system
•	Connect all instances in the terminal
•	On machine 1:
•	rpmquery nfs-utils -> which is by default on amazon Linux
•	systemctl start nfs-utils
•	systemctl status nfs-utis
•	On machine 2:
•	sudo su -
•	apt update
•	apt install nfs-common (nfs-common is name of the package in ubuntu)
•	systemctl start nfs-utils.service
•	On machine 3:
•	sudo su -
•	yum install nfs-utils -y
•	________________________________________
•	create folder in all 3 instances - mkdir /name
•	create a security group in AWS and allow port 2049 in inbound rules NFS
•	Go to instance -> security group -> Inbound rules NFS -> 2049 -> save
•	Go to EFS - > network ->and in the zone you have deployed your machines -> remove their default security group and choose your own created.also remove fatlu zones
•	-Now go to file system ( search for EFS )
•	select my-efs and attach it
•	Mount via DNS
•	sudo
•	Come to the instance and paste the DNS client command(In the EFS)nfs in the instances with /foldername --> remove the efs thing from link
•	create files in the folder of all 3 instances
•	and check ll -> all the files will be reflected in all 3 machine

################################################################################

Load balancing

•	create 4 instances in different zones - same security group
•	yum install httpd -y in all machines and /var/www/http index.html in all machines. Systemctl start/enable httpd
•	Go to load balancing
•	Create Target group
•	choose instances name it next and add the machines - include in pending create target group
•	Now go to - Load Balancer Create application load balancer
•	Name it Internet facing (for databse it should be internal) IPv4
•	select the zones in which you had made the instances (a,b,c,d)
•	select the same security group as instance
•	select http and target group then create target group
•	Now go to load balancer
•	wait for it to get active Now select the load balancer created Scroll and copy the DNS name and paste it on website
•	Now to check - go on any machine terminal and systemctl stop httpd
•	Now refresh the webpage and you will see bad gateway - later in the target group amazon will put the machine to unhealthy and stop diverting requests to it

##################################################################################

VPC peering

•	V - VPC -> cidr 10.0.0.0/16 I - IGW S - Subnet R - Route Table
•	-Search VPC
•	-Create VPC
•	Vpc Setting -> vpc only - no ipv6 cidr -> give range 10.0.0.0/16 -> create
•	FOR OHIO GIVE - 20.0.0.0/16
•	Now make IGW(internet gateway)
•	Name tag - devops-igw -> create
•	Now go to subnet -:
•	Create subnet -> -> devops-vpc -> give name -public subnet -> zone us-east-1a -> subnet cidr -> 10.0.0.0/24 -> create subnet
•	FOR OHIO 20.0.0.0/24
•	Create another subnet -> private subnet -> us-east-1b -> 10.0.1.0/24-> create
•	FOR OHIO 20.0.1.0/24
•	Now launch a instance- select devops-vpc - select public subnet -> enable auto assign public ip->allow port ssh 22 -> script if any -> launch instance
•	Launch another instance - select devops-vpc ->select private subnet -> subnet cidr -> disable auto assign public ip -> 10.0.1.0/24 -> allow 22 -> launch instance
•	Go to IGW -> select the igw -> attach to devops-vpc
•	Go to Route tables -> create -> name it -> select devops vpc -> create -> edit route -> cidr 0.0.0.0/0 and add route internet gateway -> create
•	edit subnet association -> edit and add public -> save
•	Go to instance and allow ICMP ipv4 protocol in inbound rules -> save
•	Connect the public machine in terminal
•	Copy the content from the key pair in the computer.
•	GO to the public machine vim devops-host.pem -> and paste the key save exit
•	chmod 400 devops-host.pem


NOW DO VIA VPC PEERING
GO TO NV -> PEERING CONNECTION (SEARCH VPC TO FIND IT)
•	name->nvtoohio
•	vpc Id -> your vpc id
•	region another select ohio (go to vpc in ohio and copy vpc id)
•	paste the id
•	create
•	Now go to ohio -> peering connection => you will find request (it will take time)

########################################################################

S3

Create iam user, their access key with appropriate permissions.
 
cat /etc/os-release
rpmquery mount-s3
uname -r
yum install wget -y
wget https://s3.amazonaws.com/mountpoint-s3-release/latest/x86_64/mount-s3.rpm
ll
yum install ./mount-s3.rpm
yum install unzip -y
{Paste command to install aws-cli (from aws-cli documentation) ->}
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install
l.
ls -a
cd aws
ll
cd
aws configure -> share access key -> secret key -> region: us-east-1 (or whatever rgion we're working in -> Output: Table
cd aws
ll
cd
ls -a
cd .aws/
ll
cat config
cat credentials
cd
mkdir /devops (creating a mountpoint)
mount-s3 <bucket name> /devops/ (folder name)
df -h
cd /devops/
ll
cd
