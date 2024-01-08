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
    TestTest@45677



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
http://YourUbuntuMachinePubIP:8080/

To get the password of the Jenkins machine, 
sudo cat /var/lib/jenkins/secrets/initialAdminPassword


username : admin
password : c80a7d6d9ce74e22bf002435aeccb836

Install the required plug in.

Create Admin user 
Add the e-mail and other details and save the details.

Further, create a new user as given below
    Provide your username : labuser1
    Provide password : TestTest@45677



###############################################################
Free Style project
###############################################################

Types : Freestyle/ Pipeline (Workflow) DSL / Multiconfiguration project.

Open the Jenkins Dashboard
Click on the new item - provide name .. firstfreestyle
Select Freestyle project - Click ok
In the configuration - General Description - provide
Add Build Steps - we have multiple options - Select Execute Shell option - Provide the script -- echo "hello to learning team"
Then click on the "Build now" option, notice the build runs successfully.. Notice the echo message.

###############################################################
Configure Job
###############################################################

Discard old build..
    Strategy - Log rotation
        Days to keep builds

Go through all the build config option

Cron Schedule
* * * * *
| | | | |
| | | | +----- Day of the week (0 - 6) (Sunday = 0)
| | | +------- Month (1 - 12)
| | +--------- Day of the month (1 - 31)
| +----------- Hour (0 - 23)
+------------- Minute (0 - 59)

For example, to schedule a build every day at 2:30 AM, you can use:
30 2 * * *
This means the build will trigger at 2:30 AM every day.
To trigger the build every 15 minutes:
H/15 * * * *
To trigger the build every Monday at 8:00 AM:
0 8 * * 1
You can customize the schedule based on your specific needs using the cron syntax. Adjust the values in the cron expression to match your desired schedule.

###############################################################
Execute bash script from Jenkins job
###############################################################

---------------
Insert Mode:

Once in insert mode, you can start typing your text as you would in any other text editor.
To exit insert mode and return to normal mode, press the Esc key.
Here's a summary of the steps:

To Insert Text:

Press i to enter insert mode.
Start typing your text.
To Save Changes and Exit:

Press Esc to exit insert mode and return to normal mode.
Type :wq and press Enter to save changes and exit.
To Exit Without Saving:

Press Esc to exit insert mode and return to normal mode.
Type :q! and press Enter to exit without saving.
To Save Changes (in Normal Mode):

Press Esc to ensure you are in normal mode.
Type :w and press Enter to save changes.
To Switch to Command-Line Mode:

Press Esc to ensure you are in normal mode.
Type : to enter command-line mode.

---------------
Shell script..
Create SSH connect 
    ssh labuser1@172.202.72.133
    TestTest@45677
    sudo i 
create tmp director create testscript.
mkdir tmp
cd tmp
vi testscript.sh
INSERT
echo "I am $Name, and I am $Age
ls -lrt
Modify permission of the file.
chmod 755 testscript.sh
-- validate with 
ls lrt
./testscript.sh Raj 22
vi testscript.sh

Name=$1
Age=$2
echo "Hello I am $Name, and I am $Age years old."

Execute the script from Free style job
name=Souraj
age=12
/tmp/testscript.sh $name $age

---------------

Parameterized job 

---------------

Go to configure job
Enable - This project is parameterized
Add - String parameter
    name
    default value
Add - String parameter
    age
    default value

Build steps.. use the following script.
/tmp/testscript.sh $name $age

---------
Parameterised Job - deep
----------

Exercise - add a boolean variable - create a script based on boolean value true or false it decides the visibility.


----------
###############################################################
Building application with freestyle jobs
###############################################################

Manually building the application
Steps
Download the application

Maven sample available in the Github
Check in the Jenkins git availability.
git --version

git clone -b main https://github.com/aavtraining/JenGithub.git
cd /JenGithub/Jenkins/maven-samples/single-module
 mvn --version
-- root user

https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html


 sudo su -
 sudo apt-get install maven -y
 mvn --version

 maven life cycle phases
 mvn validate
 mvn compile
 mvn test
 mvn clean install

 We have successfully validated and installed the Java project with Maven, manully, next we are going to perform the same using Jenkins.


###############################################################
Automate the above application build using Jenkins
###############################################################

Create a new free style job with name first_java_app
Go to configuration.
Source code manager
Git
Provide Git repo URL in the Job config page.
change the branch name as main from master as we are using main branch.
Save and run the build to validate the code is fetching as expected.

Next .. go to configure build..
In the build steps
Invoke top-level Maven targets
In the Goals - clean package

--- you can see .. not able to access Pom.xml

next using build steps execute as a shell command, again it get fialed..
mvn clean install

why

need to provide workspace location..
all the execution happens in the workspace ..

cd $WORKSPACE
cd JenGithub/Jenkins/maven-samples/single-module
pwd
ls
mvn clean install



we are facing issue with above configuration, going one step ahead changing the path as given below.

cd $WORKSPACE/Jenkins/maven-samples/single-module
pwd
ls
mvn clean install

We can see the build get succeeded, first build it takes more time, second build fraction of time it completes.

---- Next we can do the same with invoke with maven target option.

Add build step
Maven

Clean install
Configure - Jenkins/maven-samples/single-module/pom.xml

Jenkins/maven-samples/single-module/pom.xml

You can validate both the build happens.

----
Archive the artifacts
--------

Would like to take the artifacts in the artifact location
In post build action -> Choose the Archieve the artifacts -> Jenkins/maven-samples/single-module/target/*.jar
Run the build and see the jar file get archieved.
Artifacts are listed as part of the job
-----
Test Results
-----
Would like to take the test results in the  location
In post build action -> Choose the Test result publishing -> Jenkins/maven-samples/single-module/target/*.jar
Run the build and see the jar file get archieved.
Artifacts are listed as part of the job

-----
Test Results trends
-----
Test trend - make the additional test case..
adding negative unit test scenario and observe..

Jenkins\maven-samples\single-module\src\test\java\com\TestGreeter.java

  @Test
  public void intendfail() {
    assertTrue(true);
  }


  @Test
  public void intendfail2() {
    assertTrue(true);
  }

  @Test
  public void intendfail3() {
    assertTrue(true);
  }

  run the job - notice the test trend are changing..

-----
Long running job
-----
Initial run 4.5


Open the steps - execute shell
sleep 5m
sleep 5m
Go to the console output - stop the job - confirm yes.. 

-----
Build triggers
-----

Build trigger remotely - using xml script
Build after other project build
Build periodically
Git hook trigger
Poll scm


Poll scm

    Schedule - cron job 
        Poll frequency
        * * * * *
        every minute
Build trigger remotely - using xml script
Build after other project build
Build periodically
Git hook trigger








































