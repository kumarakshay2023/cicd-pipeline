# Jenkins Integration

## Steps for Jenkins Installation with AWS Configuration

### 1. Launch EC2 Instance: Select a `t2.medium` instance type or greater for optimal performance.

### 2. Download and Run Jenkins Setup Script: Clone the GitHub repository containing the Jenkins setup script: `git clone https://github.com/kumarakshay2023/cicd-pipeline`, navigate to the directory where the `jenkins.sh` file is located, make the script executable: `chmod 700 jenkins.sh`, and run the script to install Jenkins on your machine: `./jenkins.sh`.

### 3. Configure Security Group for Jenkins Access: After installing Jenkins on your machine, go to the **Security Groups** section of your EC2 instance in the AWS Management Console, select the security group associated with your EC2 instance, click **Edit inbound rules**, add a new inbound rule with **Type**: Custom TCP Rule, **Port Range**: `8080`, **Source**: Select **Anywhere (0.0.0.0/0)** or a specific IP range depending on your security preferences. Since Jenkins runs on port `8080`, this allows access to Jenkins via your browser.

### 4. Access Jenkins and Complete Initial Setup: After installing Jenkins on your machine, copy the initial Jenkins password provided after running the `./jenkins.sh` script (displayed in the last line of the terminal output), open your browser and navigate to `http://<your-ec2-public-ip>:8080`, paste the copied password into the Jenkins setup page to unlock Jenkins, and complete the basic setup by providing a **username**, **password**, **email address**, and **full name** for the admin user, and proceeding with the setup by installing the suggested plugins or selecting the ones you need.

### 5. Click on Create Job, then select Pipeline and start writing the script. Groovy is a scripting language used to write the code to automate the CI/CD pipeline.

### 6. Now we will write script to automate the CI/CD pipeline for java based application . 

### 8. Install Checkstyle Plugin for QA Reports

In order to generate Checkstyle reports and visualize them in Jenkins, you need to install the **Checkstyle** plugin.

#### Steps to install the Checkstyle Plugin:
1. Navigate to **Manage Jenkins** on the Jenkins dashboard.
2. Click on **Manage Plugins**.
3. Go to the **Available** tab.
4. In the search box, type **Checkstyle Plugin**.
5. Select the **Checkstyle Plugin** and click **Install without restart**.

 **Install and Configure Tomcat for Deployment**

   To set up Tomcat for deploying the application, follow these steps:

   - **Download Tomcat**:
     - Go to the official [Tomcat website](https://tomcat.apache.org/download-90.cgi) and copy the link address for the desired version.
     - Download the Tomcat ZIP file using the following command:
       ```bash
       wget <Tomcat_ZIP_URL>
       ```

   - **Unzip the Tomcat Archive**:
     - Extract the downloaded Tomcat ZIP file:
       ```bash
       unzip apache-tomcat-9.0.x.tar.gz
       ```

   - **Change Tomcat Port**:
     - In the extracted Tomcat directory, navigate to the **conf** folder:
       ```bash
       cd apache-tomcat-9.0.x/conf
       ```
     - Open the `server.xml` file for editing:
       ```bash
       nano server.xml
       ```
     - Find the following line:
       ```xml
       <Connector port="8080" protocol="HTTP/1.1" ... />
       ```
     - Change the port from `8080` to another available port (e.g., `8081`) because Jenkins is already running on port `8080`:
       ```xml
       <Connector port="8081" protocol="HTTP/1.1" ... />
       ```

   - **Set Permissions for Tomcat Startup**:
     - Go to the **bin** directory of Tomcat:
       ```bash
       cd ../bin
       ```
     - Change the permissions of the Tomcat startup script:
       ```bash
       chmod 700 *.sh
       ```

   - **Start Tomcat**:
     - Start the Tomcat server by running the following command:
       ```bash
       ./startup.sh
       ```

   - **Configure EC2 Security Group for New Tomcat Port**:
     - Go to the **Security Groups** section of your EC2 instance in the AWS Management Console.
     - Select the security group associated with your EC2 instance.
     - Click **Edit inbound rules** and add a new inbound rule:
       - **Type**: Custom TCP Rule
       - **Port Range**: `8081` (or the port you chose for Tomcat)
       - **Source**: Select **Anywhere (0.0.0.0/0)** or a specific IP range depending on your security preferences.

   Now, Tomcat will be running on the specified port (e.g., 8081), and you can deploy your application to the server.



### ----------  Script for the CI/CD pipeline fOR JAVA application -------------------------------- 

```
pipeline{
    agent any
    stages{
        stage("git clone"){
            steps{
                git url: "https://github.com/kumarakshay2023/JavaApp"
                echo "we have cloned the github repo"
            }
        }
        stage("compile the code"){
            steps{
                sh "mvn compile"
            }
        }
        stage("test"){
            steps{
                sh "mvn test"
            }
        }
        stage("qualitycheck for project"){
            steps{
                sh "mvn checkstyle:checkstyle"
                recordIssues sourceCodeRetention: 'LAST_BUILD', tools: [checkStyle()]
            }
        }
        stage("package the code"){
            steps{
                sh "mvn package"
            }
        }
        stage("deploy the app on tomcat"){
            steps{
                sh "sudo mv /var/lib/jenkins/workspace/cicdproject/target/addressbook.war /home/ubuntu/apache-tomcat-9.0.97/webapps"
            }
        }
    }
}
```

### 6. Resolve Jenkins Permission Issues (If Any)

After running the script, you might encounter an error like this:
This error is related to permission issues for the **jenkins** user on your machine. By default, Jenkins does not have the necessary privileges to execute commands with `sudo` without a password.

#### Solution:
To resolve this issue, you need to grant the **jenkins** user permission to run `sudo` commands without a password prompt:

1. Open the **sudoers** file using **visudo** to avoid syntax errors:
   sudo visudo
   Add the following line to grant the jenkins user permission to run commands as sudo without a password prompt:

Copy code
```jenkins ALL=(ALL) NOPASSWD: ALL```