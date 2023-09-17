## <ins>Deployment 3: AWS ElasticBeanstalk CLI Instance Staging Environment</ins>
_________________________________________________
  ##### September 17, 2023
______________________________________
### <ins>PURPOSE:</ins>
___________________
&emsp;&emsp;&emsp;&emsp;    To provision a Jenkins server through a newly created AWS EC2 instance with necessary protocols, permissions and downloaded applications to successfully deploy web application through the AWS Elastic Beanstalk Command Line Interface. I previously used my instructor's Jenkins browser. Establishing my own Jenkins web browser and server gives me admin access over my own Jenkins browser and the ability to customize plugins for staging my virtual environment.

&emsp;&emsp;&emsp;&emsp;     I customized my EC2 instance with the necessary security groups to establish SSH protocols for remote access between my EC2 and Jenkins web server, as well as HTTP and HTTPS protocols so that AWS ElasticBeanstalk Command Line Interface can use its resources to unencrypt my web application for deployment. I also updated my Jenkins code with a deploy stage through a multibranch pipeline to create multiple virtual environments to stage and test my application. Additionally, I created an API through a GitHub webhook to compare the different ways developers have to test and connect their web application or other applications from anywhere. 

&emsp;&emsp;&emsp;&emsp;    Utilizing the tool of the AWS EB CLI helps to launch my web application remotely through the terminal of an EC2 instance or other virtual server, and the other applications uploaded in said instace helps to automatically test my application build through Jenkins and deploy in the command line interface. Ensuring my skills to build my staging environment will help me ensure that my deployment is tested properly for high quality user experience, adequate server connection, and shows me the safest way to make sure that my web applications and servers will be ready before deployment for my future production environments.
_______________________
#### <ins> DESCRIPTION: </ins>
__________________________
&emsp;&emsp;&emsp;&emsp;        This project began diagramming the plan for my deployment in Draw.io including how I would use **GitHub**, **Jenkins**, and **AWS Elastic Beanstalk CLI**. I handled my application codes for Jenkins and Python through Github. Then I continued to create my own Jenkins server through an EC2 instance that I downloaded the latest version of Python and Jenkins on, and launched with the necessary security group(s) with the SSH and HTTP protocols for remote connection between my servers and to unencrypt my code that allows the web server to serve up my web application. Next, I built a staging environment through Jenkins with the plugins necessary to complete the CICD pipeline steps. Additionally, Jenkins allowed me to connect to my GitHub repository with the essential application codes so that I did not have to manually put them into Jenkins because they were already prepared in my repository. I authorized the connection to my Jenkins server and OS Ubuntu through my secret access key generated in AWS. Lastly, I installed AWS ElasticBeanstalk Command Line Interface onto my EC2 to deploy my application remotely from my EC2 instance terminal which is beneficial for developers in case they aren't on the premises and they can easily make changes and redeploy applications from where they currently are. With the added webhook in my last step, i was also able to automatically trigger my build test in the Jenkins server connected to the AWS EB CLI through my EC2 instance.
____________________________________
###### **Below you will find the necessary steps that I took to provision my own Jenkins server for my staging environment and to provision my production environment through Elasticbeanstalk to deploy my web application:** 

_________________________________________________________
### <ins> **SYSTEM DIAGRAM** </ins>
__________________________________________________________

<ins> ***1. Diagram the plan for deployment on Draw.io:*** </ins>

![system diagram](https://github.com/DANNYDEE93/Deployment3/blob/main/system_diagram3.jpg)

2. Log into GitHub account.

3. Create new GitHub repository.

4. Create **Documentation.md** file to record the staging and production environment of deployment.

5. Unzip files by extracting Kura Lab repository contents from the folder downloaded on local computer to separate them from their parent folder and re-compress them into a new folder excluding that parent folder so that GitHub can read it properly and upload.

6. Upload Kura Lab repository files into new GitHub repository created in **Step 3**.

___________________________________________________________________________________

   
<ins> ***Create Build in Jenkins to test application in staging environment:*** </ins>

***Jenkins is the main tool used in this deployment for pulling the program from the GitHub repository to build, test, and analyze the application before deploying***.

1. Launch an EC2 with the necessary protocols to connect to my Jenkins server and web browser and create an account with admin access to utilize my Jenkins account:

&emsp;&emsp;&emsp;&emsp;        1a. Press **Instances** in the Dashboard --> Press **Launch Instance** button--> Name web server --> Select **Ubuntu** for OS --> Select **t2.micro** --> Select suggested key pair --> Select security groups that include: Port 22[SSH], 8080[JENKINS], & 443[EB CLI] under "Network Settings" (selected existing group with these protocols) --> Press **Launch Instance**.

2. <ins> Run commands in EC2 terminal to download newest versions and packages for Pyhton 3.10:</ins>

   **sudo apt update** -->

   **sudo apt upgrade** -->

   **sudo apt install python3.10-venv** *[for the python virtual environment]* -->

   **sudo apt install python3-pip** *[includes the dependencies needed for the packages being installed]* -->

   **sudo apt install unzip**

3. <ins>Run commands in EC2 terminal to download newest versions and packages for Jenkins 2.414.1:</ins> 

   **sudo apt update** --> 
  
   **sudo apt install openjdk-17-jre** *[we need to install Java for Jenkins to install and run properly because Jenkins is written in the Java program language]* -->
  
   **java -version** *[check for current java version]* --> 
  
   **curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \ /usr/share/keyrings/jenkins-keyring.asc > /dev/null echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \ https://pkg.jenkins.io/debian-stable binary/ | sudo tee \ /etc/apt/sources.list.d/jenkins.list > /dev/null** *[to install Ubuntu requirements to allow Jenkins to run properly in EC2 instance]* --> 
  
   **sudo apt-getupdate** --> 

   **sudo apt-get install jenkins** --> 
  
   **sudo systemctl start jenkins.service** --> 
  
   **sudo systemctl status jenkins** *[checks that jenkins package is actively running on EC2]*

4. Download newest versions of  Python[3.10-venv]: Run commands: "python3.10-venv", "python-pip", "unzip"and Jenkins[2.414.1] -->

5. (*proceed to Jenkins*) Copy and Paste ip address of EC2 and add port 8080 [**ip.address:8080**] to access web browser with Jenkins. 

6. (*proceed to EC2*) The web browser will show the location of admin passwrd --> Go back to EC2 and type **"sudo cat /var/lib/jenkins/secrets/initialAdminPassword"** (*proceed back to Jenkins*):

7. Copy and paste admin password into Jenkins browser--> Create admin account-->Install suggested plugins
    
8. Press **New Item** tab to create new pipeline build --> Name pipeline --> Create multibranch pipeline --> Select GitHub through branch source --> Add Jenkins source and use GitHub credentials and token. *[multibranch pipeline allows the implemention of different Jenkinsfiles for different branches of the same project which you will need for later steps in this process where I update my Jenkins file in my repo]*
____________________________________________________
<ins> *Switch to Github* </ins> --> 9. Go back to newly created GitHub repository and copy the HTTP code or URL of the repository --> Paste code into "Repository line" in Jenkins.


<ins> ***Create token for Jenkins using GitHub account:*** </ins>

1. Go back to your GitHub and press profile picture/icon,

2. Select **Settings**-->**Developer Settings**-->**Personal Access Tokens**--> **Tokens(classic)**

3. Select **Generate new token**-->**Generate new token (classic)**-->Sign into GitHub if prompted

4. Create note description-->Select scopes: **repo** and **admin repo** to give full access to repository.
_____________________________________
<ins> *Switch back to Jenkins* </ins> --> 5. Click **Generate token**--> Copy and paste link into **password line** in Jenkins
_____________________________________

6. Continue on Jenkins-->Add token associated with repository--> *[the token is for added security and authorization and to connect GitHub repo with Jenkins to scan and test build for application]*

7. **Scan repository Now** to test build --> Select **Build History** to view console output with git commands and pipeline stage process --> Pass staging environment in Jenkins before proceeding.

***Check console output responses and check the phases of testing and passing the staging environment.***
_______________________________________________
<ins> *Switch back to AWS Instance account* </ins> --> 8. Before downloading and configuring EB CLI into EC2, you need to ensure you have the correct IAM roles to provide permissions for EB CLI to manage resources for deployment and access key credentials to enter into EC2 instance to authorize the connection. 


<ins> **Naviagting through AWS Elastic Beanstalk** </ins>


<ins>***Create IAM Roles for CLI***</ins>

1. Sign into Amazon AWS console with appropriate **Account ID, IAM user name, and password**.

2. Go to *IAM Roles* --> Click **Create Role**--> Select **AWS service** for trusted entity type-->Select **Elastic Beanstak Customizable** under cases --> Click **Next** --> Click **Next** --> Name Role: aws-elasticbeanstalk-service-role
    
3. Click **Create Role**--> Select **AWS service** for trusted entity type--> Click **EC2** under common use cases--> Click Next--> Under Permission policies: Select **AWSElasticBeanstalkWebTier**, **AWSElasticBeanstalkMulticontainerDocker** and **AWSElasticBeanstalkWorkerTier**--> Click Next--> Enter **Elastic-EC2** in **Role name** field--> Click **Create Role**.
    
***These roles allow the instances in my web server environment to access and upload necessary files with AWS resources and grants permission for Amazon Elastic Container Service to organize and cluster task within container environments.***

<ins>***Create AccessKey***</ins>

4. Go back to IAM Roles --> Go to **Users** then clikc **username** --> Go to **Security Credentials** --> Click **Create access key** --> Click **Command Line Interface(CLI)** under use cases --> Check the box under **Confirmation** --> Click **Next** --> Label description tag --> **Create access key** --> **Download** .csv file to copy access key and password -->
______________________
<ins> *Switch back to EC2 terminal and run the following commands to install AWS* </ins>
___________________________
1. Run command: **curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"** *[download zipped package of AWS to merge Elastic Beanstalk into it]*

2. Run command: **unzip awscliv2.zip** *[package to unzip files to make them accessible in instance]*

3. Run command: **sudo ./aws/install** *[install AWS]*

4. Run command: **aws configure** *[update AWS package and enter the following info from your AWS account: ]*

5. Enter your Access Key ID--> Enter Secret Access Key --> Default region: **us-east-1** --> Output format: **json** --> Run: **aws ec2 describe-instances** --> should result in **JSON output**

6. Run **sudo su -jenkins -s /bin/bash** to sign into EC2 as Jenkins user & download newest versions of AWS ElasticBeanstalk command line interface [EB CLI 3.20.9]:
______________________________________
<ins> *Continue signed in as Jenkins user in EC2 terminal to upgrade AWS package to include EB CLI:* </ins>
________________________________________
7. Run command:  **pip install awsebcli --upgrade --user**
  
8. Run command: **export PATH=$PATH:$HOME/.local/bin** *[direction to specific directory to find commands for Elastic Beanstalk]*

9. Run command: **eb version** *[checks that you have the latest version of AWS ElasticBeanstalk CLI]*

10. Run command: **aws configure** *[configure AWS package to make GitHub repo from Jenkins application available on EC2 instance with access key created for AWS EB authorization]*

11. Run command: **cd workspace** *[switch directories into Jenkins browser that has been conncected through SSH in the instance]*

12. Run command: **cd [multibranch pipeline file]** *[can switch into file to initiate deployment between Jenkins and AWS EB CLI]*

13. Run: **eb init** to initiate elastic beanstalk and set parameters for virtual environment:

14. Run: **eb create** to create environment to launch application
       *[**eb create** creates a new t3.micro instance/ server in AWS EB with the parameters entered within the **eb init** command; this new instance is helpful in case my current instance goes down, then my application can continue to be deployed on each newly created t3.micro instance]*
    *[t3. micro instance storage is more expensive thatn t2. micro but more helpful for larger applications, faster throughput and lower latency]*

_________________________________
<ins> *Switch to GitHub Jenkins file* </ins>
___________________________________________

15. Insert **stage ('Deploy') { steps { sh '/var/lib/jenkins/.local/bin/eb deploy' } }** into Jenkins file to stage deployment
    *[once this deployment stage is added to Jenkins file, we can re-deploy the application code in the AWS EB CLI to deploy my application. Once a new EB environment is created, the listed application URL will run successfully in a new browser]*

*Switch to Jenkins browser*

16. Scan repository in Jenkins to re-deploy --> Successful test

17. Repeat **steps 7-14** to redeploy application throught AWS EB CLI --> Successful

________________________________
<ins> *Configure webhook:* </ins>
__________________________________

1. Click **Settings** on your main repo page --> Click **Webhooks** on the side dashboard --> Click **Add webhook** --> Click the **Payload URL** field. --> Enter: **https//<public_ip_address:8080>.com** --> Add webhook --> Click the **http URL** --> Click on recent deliveries --> Click on hash --> Check for **successful 200 response code** [POST request to server to accept the data on the application code enclosed in the ip address of my instance  
    *[purpose of the webhook is to automatically triggers the application to the Jenkins server over the web through an API]*
    
2. Changed "Page not found".html file messagee to "Be back soon.." if an user runs into an issue with loading the page after getting a 404 error code. *[once completed, a git commit is made to my GitHub repo that triggers a new test for the build of my Jenkins project --no need to re-scan repo in Jenkins]*

3. Repeat ***steps 7-14** to redeploy application throught AWS EB CLI --> Successful

______________________________________

## <ins>**ISSUES/ OPTIMIZATION**</ins>
________________________________________
&emsp;&emsp;&emsp;&emsp;        This deployment helped me understand the different environments for staging and producing my web application on a greater and more efficient level. I ran into issues when trying to install Jenkins because I forgot the necessary steps. This could have been fixed by having them recorded in an easily accessible place on my cpu. I also had trouble with downloading AWS EB CLI. I realized that I needed to download the necessary packages in my OS Ubuntu as well as in the Jenkins user so that Jenkins can successfully utilize the command line interface. If you don't do the steps to install AWS EB CLI as the Jenkins user, eb commands will not be installed to be used to initiate and create an EB environment for deployment. 

&emsp;&emsp;&emsp;&emsp;        To optimize this type of deployment, I will create and include a Bash script that when run, it will automatically install Jenkins and another Bash script for AWS EB CLI so that i can save time on constantly installing and configuring the same applications for future deployments. 
