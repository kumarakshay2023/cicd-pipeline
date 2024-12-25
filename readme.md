# Jenkins integration 


## Steps for jenkins installation with aws configuration

### 1. Launch ec2 instance select t2medium instance type or greater than that 

### 2. Download and Run Jenkins Setup Script
Clone the GitHub repository containing the Jenkins setup script:
git clone https://github.com/kumarakshay2023/cicd-pipeline
Navigate to the directory where the jenkins.sh file is located.
Make the script executable:
chmod 700 jenkins.sh
Run the script to install Jenkins on your machine:
./jenkins.sh

### 3. Configure Security Group for Jenkins Access

After installing Jenkins on your machine, follow these steps:

Go to the Security Groups section of your EC2 instance in the AWS Management Console.
Select the security group associated with your EC2 instance.
Click Edit inbound rules.
Add a new inbound rule:
Type: Custom TCP Rule
Port Range: 8080
Source: Select Anywhere (0.0.0.0/0) or a specific IP range (e.g., your IP address) depending on your security preferences.
Since Jenkins runs on port 8080, this allows access to Jenkins via your browser.