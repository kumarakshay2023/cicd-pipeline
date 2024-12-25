# Jenkins Integration

## Steps for Jenkins Installation with AWS Configuration

### 1. Launch EC2 Instance: Select a `t2.medium` instance type or greater for optimal performance.

### 2. Download and Run Jenkins Setup Script: Clone the GitHub repository containing the Jenkins setup script: `git clone https://github.com/kumarakshay2023/cicd-pipeline`, navigate to the directory where the `jenkins.sh` file is located, make the script executable: `chmod 700 jenkins.sh`, and run the script to install Jenkins on your machine: `./jenkins.sh`.

### 3. Configure Security Group for Jenkins Access: After installing Jenkins on your machine, go to the **Security Groups** section of your EC2 instance in the AWS Management Console, select the security group associated with your EC2 instance, click **Edit inbound rules**, add a new inbound rule with **Type**: Custom TCP Rule, **Port Range**: `8080`, **Source**: Select **Anywhere (0.0.0.0/0)** or a specific IP range depending on your security preferences. Since Jenkins runs on port `8080`, this allows access to Jenkins via your browser.

### 4. Access Jenkins and Complete Initial Setup: After installing Jenkins on your machine, copy the initial Jenkins password provided after running the `./jenkins.sh` script (displayed in the last line of the terminal output), open your browser and navigate to `http://<your-ec2-public-ip>:8080`, paste the copied password into the Jenkins setup page to unlock Jenkins, and complete the basic setup by providing a **username**, **password**, **email address**, and **full name** for the admin user, and proceeding with the setup by installing the suggested plugins or selecting the ones you need.

### 5. Click on Create Job, then select Pipeline and start writing the script. Groovy is a scripting language used to write the code to automate the CI/CD pipeline.
