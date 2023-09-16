## <ins>Deployment 3: AWS ElasticBeanstalk CLI Instance Staging Environment for Deployment</ins>
_________________________________________________
  ##### September 15, 2023
______________________________________
### <ins>PURPOSE:</ins>
___________________
&emsp;&emsp;&emsp;&emsp;    To provision a Jenkins server through a newly created AWS EC2 instance with necessary protocols, permissions and downloaded applications to successfully deploy web application. I previously used my instructor's Jenkins browser. Establishing my own Jenkins web browser and server gives me admin access over my own Jenkins browser and the ability to customize plugins for staging my virtual environment.

&emsp;&emsp;&emsp;&emsp;    Even though my AWS account already includes the correct security group protocols, creating and launching my own EC2 instance also provides me the ability to customize my own instance with the necessary security groups to establish SSH protocols for remote access between my EC2 and Jenkins web server, as well as HTTP protocols so that AWS can use its resources to unencrypt my web application for deployment through my public IP address from my EC2 instance.

&emsp;&emsp;&emsp;&emsp;    Learning these fundamental steps gives me practice in how to start the steps in procuring my CICD pipeline for automating and scaling my deployment process in the future. Ensuring my skills to build my staging environment will help me ensure that my deployment is tested properly for high quality user experience, adequate server connection, and shows me the safest way to make sure that my web applications and servers will be ready before deployment for my future production environments.
_______________________
#### <ins> DESCRIPTION: </ins>
__________________________
&emsp;&emsp;&emsp;&emsp;        This project began diagramming the plan for my deployment in Draw.io including how I would use **GitHub**, **Jenkins**, and **AWS Elastic Beanstalk**. I handled my application codes for Jenkins and Python through Github. Then I continued to create my own Jenkins server through an EC2 instance that I downloaded the latest version of Python and Jenkins on, and launched with the necessary SSH and HTTP protocols for remote connection between my servers and to unencrypt my code that allows the web server to serve up my web application. Next, I built a staging environment through Jenkins with the plugins necessary to complete the CICD pipeline steps. Additionally, Jenkins allowed me to connect to my GitHub repository with the essential application codes so that I did not have to manually put them into Jenkins because they were already prepared in my repository. 

&emsp;&emsp;&emsp;&emsp;        Once my application code passed the building test phase in Jenkins, I created IAM roles and a Python URL shortener virtual environment to build, test and deploy my application through Elastic Beanstalk. This allowed AWS access with the necessary permissions to update and launch my deployment through the IAM roles of **AWSElasticBeanstalkWebTier**, **AWSElasticBeanstalkMulticontainerDocker** and **AWSElasticBeanstalkWorkerTier**. The URL shortener was utilized so that the resources from AWS could easily machine-read the files I attached with my application code-- essentially compressing my URL so that it could fit neatly into Elastic Beanstalk's requirements for deployment within the virtual production environment I created. 
____________________________________
###### **Below you will find the necessary steps that I took to provision my own Jenkins server for my staging environment and to provision my production environment through Elasticbeanstalk to deploy my web application:** 

## <ins> **ISSUES** </ins>

_________________________________________________________

## <ins> **SYSTEM DIAGRAM** </ins>


<ins> ***1. Diagram the plan for deployment on Draw.io:*** </ins>

![system diagram](https://github.com/DANNYDEE93/Deployment3/blob/main/system_diagram.jpg)

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

2. Within my EC2 instance: Download newest versions of  Python[3.10-venv] and Jenkins[2.414.1] -->

3. (*proceed to Jenkins*) Copy and Paste ip address of EC2 and add port 8080 [**ip.address:8080**] to access web browser with Jenkins. 

8. (*proceed to EC2*) The web browser will show the location of admin passwrd --> Go back to EC2 and type **"sudo cat /var/lib/jenkins/secrets/initialAdminPassword"** (*proceed back to Jenkins*):

9. Copy and paste admin password into Jenkins browser--> Create admin account-->Install suggested plugins
    
    ![Pipleline](https://github.com/DANNYDEE93/Deployment2/blob/main/Images%20of%20Deployment%202/Download%20plugins%20for%20environment.png)
   
10. Press **New Item** tab to create new pipeline build --> Name pipeline --> Create multibranch pipeline --> Select GitHub through branch source --> Add Jenkins source and use GitHub credentials and token.
____________________________________________________
*Switch to Github* --> Go back to newly created GitHub repository and copy the HTTP code or URL of the repository --> Paste code into "Repository line" in Jenkins.

<ins> ***Create token for Jenkins using GitHub account:*** </ins>

11. Go back to your GitHub and press profile picture/icon,

12. Select **Settings**-->**Developer Settings**-->**Personal Access Tokens**--> **Tokens(classic)**

13. Select **Generate new token**-->**Generate new token (classic)**-->Sign into GitHub if prompted

14. Create note description-->Select scopes: **repo** and **admin repo** to give full access to repository.
_____________________________________
*Switch back to Jenkins* --> Click **Generate token**--> Copy and paste link into **password line** in Jenkins
_____________________________________

15. Continue on Jenkins-->Add token associated with repository-->

16. **Scan repository Now** to test build --> Select **Build History** to view console output with git commands and pipeline stage process --> Pass staging environment in Jenkins before proceeding.

***Check console output responses and check the phases of testing and passing the staging environment.***
_______________________________________________
*Switch back to AWS Instance account*

14. Before downloading and configuring EB CLI into EC2, you need to ensure you have the correct IAM roles and access key to allow EB CLI to manage resources for deployment.

<ins> **Navigating through AWS Elastic Beanstalk** </ins>

<ins>***Create IAM Roles for CLI***</ins>

1. Sign into Amazon AWS console with appropriate **Account ID, IAM user name, and password**.

2. Go to IAM Roles --> Click **Create Role**--> Select **AWS service** for trusted entity type-->Select **Elastic Beanstak Customizable** under cases --> Click **Next** --> Click **Next** --> Name Role: aws-elasticbeanstalk-service-role
    
3. Click **Create Role**--> Select **AWS service** for trusted entity type--> Click **EC2** under common use cases--> Click Next--> Under Permission policies: Select **AWSElasticBeanstalkWebTier**, **AWSElasticBeanstalkMulticontainerDocker** and **AWSElasticBeanstalkWorkerTier**--> Click Next--> Enter **Elastic-EC2** in **Role name** field--> Click **Create Role**.
    
***These roles allow the instances in my web server environment to access and upload necessary files with AWS resources and grants permission for Amazon Elastic Container Service to organize and cluster task within container environments.***

<ins>***Create AccessKey***</ins>

1. Go back to IAM Roles --> Go to **Users** then clikc **username** --> Go to **Security Credentials** --> Click **Create access key** --> Click **Command Line Interface(CLI)** under use cases --> Check the box under **Confirmation** --> Click **Next** --> Label description tag --> **Create access key** --> **Download** .csv file to copy access key and password -->
______________________
**Switch back to EC2*
___________________________
2. Run command: **curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"**
3. Run command: **unzip awscliv2.zip**
4. Run command: **sudo ./aws/install**
5. Run command: **sudo ./aws/install**
6. Run command: **aws configure**

7. Enter your Access Key ID--> Enter Secret Access Key --> Default region: **us-east-1** --> Output format: **json** --> Run: **aws ec2 describe-instances** --> should result in **JSON output**

8. Use **sudo su -jenkins -s /bin/bash** to sign into EC2 as Jenkins user & download newest versions of AWS ElasticBeanstalk command line interface [EB CLI 3.20.9]:

9. Run command: **sudo ./aws/install**
10. Run command: **sudo ./aws/install**
11. Run command: **aws configure**

Use access key to

Insert **stage ('Deploy') { steps { sh '/var/lib/jenkins/.local/bin/eb deploy' } }** into Jenkins file to stage deployment

Scan repository in Jenkins to re-deploy --> Successful test
_______________________

Configure webhook:

___________________

Changed "Page not found".html to "Be back soon.."

Scan repository in Jenkins to re-deploy --> Successful test


______________________________________

## <ins>**OPTIMIZATION**</ins>
________________________________________
&emsp;&emsp;&emsp;&emsp;        This deployment helped me understand the different environments for staging and producing my web application on a greater level. To optimize the efficacy of this deployment in the future, I would take more screenshots of each process to make it easier for other developers to reference my process of deployment in order to attempt it themselves. Lastly, instead of repeating steps from previous repositories within my documentation/readme.md file, I would just reference the other repositories that include the certain steps that I continuously repeat. This can help me utilize my time better to focus on explaining the different aspects of my deployments that I have not done before. 
