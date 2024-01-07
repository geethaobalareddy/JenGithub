Know about Lab machine, Azure Pass and Jenkins machine
Setup a Github account
Setup a Microsoft hotmail account 
Using Microsoft account login Azure Pass and claim the azure Pass

###############################################################
Setup Lab machine
###############################################################
Login windows machine
Install VS code on Lab machine
Install Chrome browser



###############################################################
Create Jenkins machine using Azure PaaS
###############################################################

1) Login Portal.azure.com
2) create Azure - Ubuntu 22 version machine
    Provide your username : labuser1
    Provide password : TestTest@45677

To access port 8080 based Jenkins server from this machine, open the created VMs Network settings, create a port rule add port number 8080 and other default setting and add a new rule.

Open VS Code
Open the VS Code Terminal
Connect the newly created Ubuntu machine using following commmand
    ssh labuser1@172.202.72.133



###############################################################
2) Install Jenkins
###############################################################
In chrome browser of lab machine search for Jenkins download and you will given below URL, choose Ubutu, we have all the given below commands available for the Jenkins setup.
    https://www.jenkins.io/download/
    https://pkg.jenkins.io/debian-stable/


    sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
    https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key

      echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null


    sudo apt-get update
    sudo apt-get install fontconfig openjdk-17-jre
    sudo apt-get install jenkins

    Go through the logs

sudo systemctl status jenkins
sudo systemctl enable jenkins
lsb_release -r
Release:        22.04

After above Jenkins validation access the Jenkins machine from public browser using following URl
http://172.202.72.133:808/

To get the password of the Jenkins machine, 
sudo cat /var/lib/jenkins/secrets/initialAdminPassword

c80a7d6d9ce74e22bf002435aeccb836

Further, create a new user as given below
    Provide your username : labuser1
    Provide password : TestTest@45677




