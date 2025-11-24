# This is a Documentation On The Automation Of Deploying A Simple Html Webpage Using AWS Ec2 Instance


### Prerequisites
EC2 Instance Specifications:

Instance Type: t3.small  
vCPUs: 2  
Memory: 2 GiB  
Operating System: Ubuntu 22.04 LTS  
Cloud Provider: AWS EC2  
Public IP: 13.51.69.106

My Security Group was configured this way
![](./Images/28.png)



## Jenkins Installations
#### Step 1.1: First I did SSH into my EC2 instance using my private key.  

#### Step 1.2: Update and upgraded the server, this ensures all system packages are up to date before the installation of new software.
![](./Images/02.png)

#### Step 1.3: Install Java  
Jenkins is built on Java and requires Java Runtime Environment to operate
![](./Images/03.png)

To verify
![](./Images/03i.png)

#### Step 1.4: Add Jenkins Repository and Install

![](./Images/04.png)

#### Step 1.5: Start and Enable Jenkins Service
![](./Images/04i.png)

#### Step 1.6: Access Jenkins Web Interface
![](./Images/05.png)


#### Step 1.7: Retrieve Initial Admin Password

![](./Images/05i.png)

Now Install Suggested Plugins Create User Account and Launch Jenkins!

### Plugin Configuration
Install sugessted plugin and install, then create User Admin account.  
Add and  Install Additional Required Plugins which is required for this project.
![](./Images/06.png)



### Security Measures

Have to make the following configurations on Jenkins:

#### Authentication Configuration

Security Realm: Jenkins' own user database
Purpose: Manages user authentication internally

#### Authorization Configuration
Authorization Strategy: Logged-in users can do anything

![](./Images/07.png)

### Docker Integeration
Install Docker repository and Install on EC2

![](./Images/08.png)

Confirm Docker is installed
![](./Images/08i.png)

Add Jenkins User to Docker Group: This allows Jenkins to execute Docker commands without sudo  
Restart Jenkins and check to verify the status of Jenkins.
![](./Images/09.png)

Verify Docker Installation
![](./Images/10.png)


### Git Integration
Insall and verify git installation

![](./Images/11.png)



## Source Code Management Repository Integration

Navigate to home directory and make a new directory for your project

![](./Images/12.png)
 Create an HTML Application file and paste in the following script - Make your own modification

![](./Images/12i.png)
![](./Images/12ii.png)

#### Create  Dockerfile

![](./Images/13.png)

#### Initialize Git Repository
![](./Images/14.png)
Initialize a git local repository
![](./Images/15.png)

![](./Images/15i.png)

Navigate to https://github.com/ and create a github repository and make it public.
![](./Images/16.png)
Back on your server, connect and push to Github.
![](./Images/16i.png)
![](./Images/16ii.png)
![](./Images/16iii.png)
![](./Images/16iv.png)
![](./Images/16v.png)


## Jenkins Freestyle Project

Create New Freestyle Job
![](./Images/17.png)

Configure the settings
![](./Images/18.png) 


Source Code Management  ![](./Images/18i.png)

Jenkins Credentials ![](./Images/18ii.png)

Git ![](./Images/18iii.png)

Triggers![](./Images/18iv.png)

Build Steps ![](./Images/20.png)   


Post Build Actions![](./Images/21.png)


Add Jenkins user to the Docker Group ![](./Images/19.png)

Trigger the Build Process and watch out for this display on the Console Output ![](./Images/22.png)


Configure Github Webhook ![](./Images/24.png)

Verify Application running on browser ![](./Images/23.png)





## Jenkins Pipeline 
Create Jenkinsfile ![](./Images/25.png)

Jenkinsfile ![](./Images/25i.png)

Push to git repo ![](./Images/26.png)



#### Configure Jenkins Pipeline

Create pipeline ![](./Images/27.png)

Add Github project ![](./Images/27i.png)

![](./Images/27ii.png)

Branch and Script Path and Save ![](./Images/27iii.png)


Click on build now to trigger the deployment![](./Images/27iv.png)

We initially access the **Jenkins Freestyle Application** running on port 8080, Now to access the **Jenkins Pipeline Application** we access that on port 8081.
![](./Images/29.png)

Jenkins Freestyle Application deployed successfully. 



## Docker Image Creation And Push To Registry

Docker registries serve as centralized repositories for storing and distributing Docker images. This component automates the process of building Docker images and pushing them to Docker Hub, enabling consistent deployments across different environments and facilitating image versioning.


Docker Hub Creation  
Navigate to Docker hub and create a new Repository for the app
![](./Images/30.png)

Back on Jenkins, Create a new credential and add for Docker. This is to secure authentication for Jenkins to push images. 
![](./Images/31.png)

Update Jenkinsfile with Docker Build Image

![](./Images/32.png)


Then push updated Jenkinsfile

![](./Images/32i.png)

Push to Docker Hub and verify

![](./Images/32ii.png)

Verify on Command line with `docker pull`
![](./Images/33.png)


### DOCKER IMAGE CREATION AND PUSH SUCCESSFULL!!!