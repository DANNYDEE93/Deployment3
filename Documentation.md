## <ins>Deployment 2: Jenkins EC2 Instance Deployment</ins>
_________________________________________________
  ##### August 23, 2023
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

_____________________________________
## <ins> **PLAN & CODE** </ins>
_______________________

<ins> ***1. Diagram the plan for deployment on Draw.io:*** </ins>


![Plan](https://github.com/DANNYDEE93/Deployment2/blob/main/Images%20of%20Deployment%202/Plan%20for%20deployment%202.jpg)
   
2. Log into GitHub account.

3. Create new GitHub repository.

4. Create **Documentation.md** file to record the staging and production environment of deployment.

5. Open Kura Labs c4 Deployment-1 repository and follow the provided instructions.

6. Press the **Code** tab and download files from Kura Labs repository.

7. Unzip files by extracting Kura Lab files from the folder downloaded on local computer to separate them from their parent folder and re-compress them into a new folder excluding that parent folder so that GitHub can read it properly and upload.

8. Upload Kura Lab repository files into new GitHub repository created in **Step 3**.
_______________________________

## <ins> **BUILD & TEST** </ins>
_________________________
   
<ins> ***Create Build in Jenkins to test application in staging environment:*** </ins>

***Jenkins is the main tool used in this deployment for pulling the program from the GitHub repository to build, test, and analyze the application before deploying***.

1. Launch an EC2 with the necessary protocols to connect to my Jenkins server and web browser and create an account with admin access to utilize my Jenkins account on my own:

&emsp;&emsp;&emsp;&emsp;        1a. Press **Instances** in the Dashboard --> Press **Launch Instance** button--> Name web server --> Select **Ubuntu** for OS --> Select **t2.micro** --> Select suggested key pair --> Select security groups that include: Port 22, 80 & 8080 under "Network Settings" (selected existing group with these protocols) --> Press **Launch Instance**.

2. Within my EC2 instance: Download newest version of Python and Jenkins --> (*proceed to Jenkins*) Copy and Paste ip address of EC2 and add port 80 [**ip.address:80**] to access web browser with Jenkins. 

3. (*proceed to EC2*) The web browser will show the location of admin passwrd --> Go back to EC2 and type **"sudo cat /var/lib/jenkins/secrets/initialAdminPassword"** (*proceed back to Jenkins*):


   ![Jenkins access](https://github.com/DANNYDEE93/Deployment2/blob/main/Images%20of%20Deployment%202/sudo%20cat%20admin%20psswrd.png)

4. Copy and paste admin password into Jenkins browser--> Create admin account-->Install suggested plugins --> Go to **Available plugins** in the settings--> Install **Pipeline Utility Steps** --> Soft restart the browser --> Sign back into Jenkins account:

   
    ![Pipleline](https://github.com/DANNYDEE93/Deployment2/blob/main/Images%20of%20Deployment%202/Download%20plugins%20for%20environment.png)
   
5. Press **New Item** tab to create new pipeline build --> Name pipeline "First name, Last name initial" --> Select Pipeline script from SCM --> Choose Git in the dropdown from SCM.
____________________________________________________
*Switch to Github* --> Go back to newly created GitHub repository and copy the HTTP code or URL of the repository --> Paste code into "Repository line" in Jenkins.

<ins> ***Create token for Jenkins using GitHub account:*** </ins>

6. Go back to your GitHub and press profile picture/icon,

7. Select **Settings**-->**Developer Settings**-->**Personal Access Tokens**--> **Tokens(classic)**

8. Select **Generate new token**-->**Generate new token (classic)**-->Sign into GitHub if prompted

9. Create note description-->Select scopes: **repo** and **admin repo** to give full access to repository.
_____________________________________
*Switch back to Jenkins* --> Click **Generate token**--> Copy and paste link into **password line** in Jenkins

10. Continue on Jenkins-->Add token associated with repository--> Select main branch

11. **Submit** to generate build --> Select **Build Now** --> Click **Continue** in packaging phase --> Pass staging environment in Jenkins.

***Check console output responses and check the phases of testing and passing the staging environment.***
_______________________________________________
*Unzipping application code file in EC2 instance*

12. For this deployment, there was an included packaging/zip command within my application code that compressed my GitHub file. Under the packaging phase in the console output, the location of the file within the EC2 home directory was given:


  ![1](https://github.com/DANNYDEE93/Deployment2/blob/main/Images%20of%20Deployment%202/Jenkins%20compressed%20file%20location%20.png)

***Locating the file and unzipping the package makes it easily readable by virtual managed services from AWS, you can make sure that it was properly packaged, and creates a shorter absolute access path for each file.***

13. Go to EC2 and **cd /var/lib/jenkins/workspace/Deployment2/build/1.0.0.1.zip**

14. Download unzip: **sudo apt unzip** --> then use the "unzip" command on the "1.0.0.1.zip" file to extract files through the EC2:


  ![2](https://github.com/DANNYDEE93/Deployment2/blob/main/Images%20of%20Deployment%202/Jenkins%20compressed%20zip%20file_2.png)

_______________________

## <ins> **MERGE** </ins>
___________________

<ins> ***Download GitHub Repository to unzip files and re-zip them to upload onto AWS Elastic Beanstalk*** </ins>

1. Create zip file folder in your File Explorer **[Windows OS]** to compress your GitHub repository. This new compressed zip file should not exceed 500 MB and should not include parent folder from your original repository to ensure that all characters in file are configured correctly in Elastic Beanstalk. You will need to extract files from the folder and create a new compressed zip file like in <ins>**Step 7** under **PLAN & CODE**</ins>.

2. Select files within the downloaded file of your repository and transfer them into a new compressed zip folder. This ensures that the files do not include the parent folder and is compressed to Elastic Beanstalk's standards for reading a source bundle.
 
*After checking the console output responses and passing the test phase in Jenkins, you can go on to creating **IAM Roles** and the **Python Url Shortener** through **AWS Elastic Beanstalk***.

___________________________________________
## <ins>**BUILD, TEST, DEPLOY**</ins>
_______________________________

<ins> **Navigating through AWS Elastic Beanstalk** </ins>

<ins>***Create IAM Roles***</ins>

1. Sign into Amazon AWS console with appropriate **Account ID, IAM user name, and password**.
    
2. Select **Roles** in Dashboard--> Click **Create role**--> Select **AWS service** for trusted entity type--> Under the dropdown for AWS Services: Select **Elastic Beanstalk**--> Select **Customizable** option under cases to give Elastic Beanstalk permission to manage AWS resources of your deployment--> Click **Next**--> Click **Next**--> Enter **AWS-Elasticbeanstalk-service-role** in Role name--> Click **Create Role**.
    
3. To create the second and third role: Click **Create Role**--> Select **AWS service** for trusted entity type--> Click **EC2** under common use cases--> Click Next--> Under Permission policies: Select **AWSElasticBeanstalkWebTier**, **AWSElasticBeanstalkMulticontainerDocker** and **AWSElasticBeanstalkWorkerTier**--> Click Next--> Enter **Elastic-EC2** in **Role name** field--> Click **Create Role**.
    
***These roles allow the instances in my web server environment to access and upload necessary files with AWS resources and grants permission for Amazon Elastic Container Service to organize and cluster task within container environments.***

<ins>***Create and deploy a Python URL shortener***</ins>

4. Select **Create Application**--> Name application **URL-shortener**--> Select **Python** in Platform dropdown--> Select **Python 3.9 running on 64bit Amazon Linux 2023** in Platform branch dropdown-->Select **Upload your code**--> Type **V1** in version label field--> **Upload** the compressed zip file of your repository -->Click **Next**.

5. Select **ElasticEC2** in EC2 instance profile dropdown--> Click **Next**--> Select default VPC in VPC dropdown--> Select **us-east-1a** availability zone--> Click **Next**--> Select **General Purpose (SSD)** in Root Volume type drop down--> Change the size field to 10GB--> Select **ONLY "T2.MICRO"** under **Instance Types** and **DESELECT** any other choices--> Click **Next**--> Click **Next**--> Click **Submit**.
    
***The URL shortener reduces the number of characters in a URL so that it is easier for Elastic Beanstalk to read, remember, and share your web service and application. AWS Elastic Beanstalk makes it easier for developers to quickly deploy and manage these applications.***

6. Wait for **Elastic Beanstalk** to launch virtual production environment:


![success1](https://github.com/DANNYDEE93/Deployment2/blob/main/Images%20of%20Deployment%202/successful%20environment%20launch.png)

7. Click **Upload and Deploy** --> Upload newly compressed zip file from <ins>**Step 1** under **MERGE**</ins> --> Wait for **Elastic Beanstalk** to upload application code, create correct application version and deploy web application -->Copy & Paste **domain URL** (*as seen in the previous image attachment under health status*) into a new browser to further check for successful deployment of my web application:


![success2](https://github.com/DANNYDEE93/Deployment2/blob/main/Images%20of%20Deployment%202/successful%20deployment%20of%20web%20app.png) 

____________________________________
## <ins>TROUBLESHOOTING:</ins>
____________________________________
&emsp;&emsp;&emsp;&emsp;        My first attempt to create my Jenkins account, I skipped the option to create "Admin Username and Password". I was able to upload my build but my test was unsuccessful. In order to troubleshoot this issue, **Steps 1-5** under **BUILD & TEST**. I had to create a new EC2 instance, establish a new connection with my Jenkins server, get admin password from EC2 to create a new Jenkins admin account, install the necessary "Pipeline Utility Steps" on top of the suggested plugins, and connect to my GitHub repository to upload my Jenkins code to build and test my application in a staging environment.

______________________________________
## <ins>CONCLUSION:</ins>
________________________________________
&emsp;&emsp;&emsp;&emsp;        This deployment helped me understand the different environments for staging and producing my web application on a greater level. To optimize the efficacy of this deployment in the future, I would take more screenshots of each process to make it easier for other developers to reference my process of deployment in order to attempt it themselves. Lastly, instead of repeating steps from previous repositories within my documentation/readme.md file, I would just reference the other repositories that include the certain steps that I continuously repeat. This can help me utilize my time better to focus on explaining the different aspects of my deployments that I have not done before. 
