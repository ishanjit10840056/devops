Day 7



AMAZON WEB SERVICES



**NIST** - Makes standards for cloud computing

**IANA** - it maintains IP address

Amazon own a hypervisor named **Nitro**



WHY CLOUD ?

1. Cut down Capex ( Capital Expenditure )
2. OPEX (operational cost)
3. Cost of Infrastructure
4. Space
5. Handling data / servers
6. Hiring system



GITOPS is also a method for CI/CD pipeline



Two types of scaling

1. Vertical Scaling  --> adding resources to a single machine
2. Horizontal scaling --> adding more machines to handle scalibility



Manual scaling

Automatic / Dynamic scaling

Schedule scaling



In Azure auto scaling is called VMSS -> virtual machine scale sets



SLA - service level agreement



Types of cloud services

1. IAAS --> ec2, ebs, vpc
2. PAAS --> ecs, elastic beanstalk, Elastic Kubernetes service (EKS)
3. SAAS --> gmail, linkedin
4. FAAS



Cloud models

1. Public
2. Hybrid
3. Private



Amazon gives Default network - VPC (virtual private cloud) i.e., the network setting of aws

VPC helps in maintaining private cloud service in a public cloud - which helps in no data sharing between multiple hosted cloud instances.



IPsec helps in maintaining connection.



Open-stack helps in creating a private cloud. OpenStack is maintained by RedHat.



AWS has 117 availability zone and 37 Regions





1 region is divided into atleast 3 zones.



Security group inherit on ec2 lan card



2/2 check - hardware and software both are checked and okay



ec2-user is part of wheel group by default





==================================================================



Create a instance in aws

set hostname

yum install httpd -y

rmpquery httpd

cd /var/www/html

cat > index.html

cd

systemctl start httpd

systemctl enable httpd



allow port 80 in inbound rules -> http

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



go to AMI's catalog to see multiple ami's



go to the instance hosted (shut-down and then do) -

select the instance

click on actions

image and templates

create a image

go to create image

Keep the reboot instance to unticked

delete on termination - untick

then click on create AMI



Now go to AMI and you will see your AMI created

You can terminate the old instance ( if delete on termination was unticked)







create new instance -  select my ami and you will see your ami

select it and go ahead



copy the new ip and paste it on brower ip:80 and you will see the existing file that you created



you can also check on console or terminal for the files, users, services installed - all will be present



To share your AMI with other -

select the AMI

Actions

Edit permission

Add Shared accounts -> add account id

Now save changes

The other person can see you AMI in private ami's





Q1. How can you migrate a server from one region to another region



Go to images - AMIs

select the AMI

Click on actions -> copy AMI

Choose new destination region ex. Ohio

click on copy AMI

and switch to Ohio region from the main menu drop down



Check the AMI image -> and check on status until it becomes available

AMI is a region based service.



Again go to AMI - select the AMI scroll down and click on permissions

Then edit AMI permission

Add account id and paste the AWS account id of the one who you want to share

Then apply



search on private ami and you will find it



Then deploy an instance using that AMI





If you want to delete - deregister the AMI and delete the snapshot



HOW TO DEPLOY A EC2 INSTANCE PROPERLY



Templates -> to create a pre-specified ec2 instance

GO to instance -> launch template

template tags

Select AMI -> amazon Linux

instance type - t2.micro

key pair - generate or select existing

select network subnet - availability zone - security groups ( create or select )

Storage 10gbs - GP2

Terminate to delete automatically

user data -> write commands to execute automatically

Launch template



Go to launch templates

select -> actions -> launch instance from template

Number of instances - 2 ( to create 2 instance at once )

Launch instance



To modify template

Select the template from launch templates - actions - modify template

and change the needed



Reserved instances are used in production. ( we can reserve instance for long term for the company )



SAN - Storage area network (image in copy)

In SAN we have block devices like - SSD, HDD



Launch and connect instance

run command - lsblk -> to check storage and partition



GO to elastic block store in aws

volumes - name it to vol1

then create new volume (make sure to create in same availability zone)

Select GP2 - size 5gb



Now go to EBS - volumes

select the new generate volume - action -> attach

select instance hosted name

attach





Go to console

lsblk -fs

blkid



file system command

mkfs.ext4 /dev/xvdb  --> formats the new disk - a for disk 1 b for disk 2 ..

blkid



create a mount point

mkdir /data

mount /dev/xvdb /data/

df -h - to check if storage is mounted



cd /data/

touch Sanjaya.txt{1..100} -> to create 100 files



THIS WAS TEMPORARY MOUNTING



TO MAKE IT PERMANENT-.

cd



blkid -> copy the uuid of partition b

open

vim /etc/fstab/

paste the uuid - to mount it permanenetly or by name - /dev/xvdb	/data	ext4	defaults	0 0

it means mount xvdb to data folder using ext4 file system



RESTART THE INSTANCE



Q2. CREATE A INSTANCE WITH VOLUME THAT VOLUME WILL BE AVAILABLE IN DIFFERENT ZONE TOO

create a volume

then create snapshot of the volume

then go to snapshot - create volume from snapshot

and select new availability zone

