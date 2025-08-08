Day 4



Jenkins

Open source platform which help in creating CI/CD pipeline

Jenkins software is developed in Java

Jenkins is used in default port 8080



why we use multiple resource

To have single point of failure





Types of Pipeline :

Declarative approach

UI based pipeline



Jenkins alternative - Gitlab, circleCI, Atlassian bamboo, Team city



Continuous delivery is a manual process 

whereas Continuous deployment is a automatic process



To build a java application we use Maven plugin in Jenkins



Steps for Jenkins



* Host aws instance -us east 1-a
* space 15gb gp2
* connect in aws machine in the terminal 
* sudo su -
* set hostname
* yum install jenkins (to install Jenkins) (it is not present in yum repository)
* get the Jenkins install command from website - https://www.jenkins.io/doc/tutorials/tutorial-for-installing-jenkins-on-AWS/#:~:text=This%20tutorial%20walks%20you%20through%20the%20process%20of,abilities%20need%20to%20be%20augmented%20on%20the%20instance.  (Jenkins on aws)
* yum repolist all (to see all repositories present in machine)
* import keyfile from website
* sudo yum upgrade
* install java (java amazon-corretto)
* sudo yum install jenkins -y (now you can install Jenkins)
* sudo systemctl enable jenkins
* sudo systemctl start Jenkins
* Add port 8080 in security group inbound rules custom TCP
* ip:8080 on google
* sudo cat /var/lib/Jenkins/secret/intialAdminPassword
* then customize Jenkins on browser - install suggested plugins
* Go to build-in-node and bring it back online
* Integrate Jenkins with GitHub repository
* Go to GitHub created repo setting - add webhook
* Paste Jenkins ip along with port/github-webhook/
* content type application/json
* Generate token in jekins security - add a token name anything generate token  \& copy token
* paste in webhook secret \& add the webhook - seeing the green tick
* save the token somewhere
* Install git in Jenkins machine



CREATE NEW MACHINE

* create dev-server machine in aws and connect it in terminal (sudo su -,sethostname)
* install git
* create a html file in a directory gi init - git add . - git commit
* git branch -M branchname(main) --to connect with the branch of get repo
* git remote add origin git@githubsshkey
* git remote -v
* ssh-keygen
* go to .ssh/ and copy the content of .pub file
* and add it to SSH and GPG keys and new ssh key and paste the key and add ssh key
* git push origin main (--force) (not to do force push)
* generally to create another branch and merge it to the main branch



Node low space issue 

sudo mount -o remount,size=2G /tmp

open vim /etc/fstab

tmpfs /tmp tmpfs defaults,noatime,mode=1777,size=2G 0 0









Now move to Jenkins

* create job - new item
* job type freestyle
* source code management Git - paste http url - 
* branch /main
* trigger manual
* then apply save
* Then click on Build Now to create a workspace
* make edit on the html file ( on dev server or 
* again click on build now to make changes
* configure trigger - click on GitHub hook trigger (auto trigger when changes in code is made) !!!Poll SCM means it works continuously so higher cpu or disk utilization
* Now make changes in the html file and add, commit, push and auto build will be scheduled and check file automatically change will be reflected









SLOW JENKIN ISSUE SOLVER

cd /var/lib/Jenkins

vim jenkins.model.JenkinsLocationConfiguration.xml

change ip and save - then reboot aws

