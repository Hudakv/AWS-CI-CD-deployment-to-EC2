# AWS CI/CD Deployment to EC2

A demo Node.js application deployed using AWS CodePipeline, CodeBuild, and CodeDeploy on an EC2 instance.

## Services used:
- AWS CodePipeline
- AWS CodeBuild
- AWS CodeDeploy
- EC2 (t2.micro)
- IAM Role for deployment
STEP 1 — Launch EC2

Open AWS Console
Search EC2
Click Launch Instance

Choose:
Amazon Linux 2
Instance type: t2.micro (Free Tier eligible)
Create or select a Key Pair (Deploy .pem)

Network settings:
Allow SSH
Allow HTTP
Click Launch

STEP 3 — Write the web application code

This is the step where you actually coded the app.
In VS Code:
Click New File
Name it: server.js
Paste this code inside it:
const http = require('http');
const server = http.createServer((req, res) => {
  res.writeHead(200, { 'Content-Type': 'text/plain' });
  res.end('Hello from AWS CI/CD Demo!');
});
const PORT = 3000;
server.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});

#Initialize npm (for Node project)

#This creates package.json which describes your project settings.

npm init -y
node server.js
If it prints:
Server running on port 3000
Then open your browser and visit:
http://localhost:3000

version: 0.2
phases:
  install:
    commands:
      - npm install
  build:
    commands:
      - echo "Build completed"
artifacts:
  files:
    - '**/*'


STEP 2 — Connect to EC2 from Mac Terminal
chmod 700 your-key.pem
ssh -i deploy.pem ec2-user@<your-ec2-public-ip>
STEP 3 — Install Node & CodeDeploy agents
sudo yum update -y
sudo yum install nodejs -y   #installing node js

# Install CodeDeploy agent
sudo yum install ruby wget -y # Use: Installs two system packages on Amazon Linux using the YUM package manager                                                               #ruby → required by the CodeDeploy installer script
                              #wget → a tool to download files from the internet
                              # The CodeDeploy installer won’t run unless Ruby exists
                               
cd /home/ec2-user             
wget https://aws-codedeploy-ap-south-1.s3.amazonaws.com/latest/install   #Downloads the latest CodeDeploy agent installer script from AWS’s S3                                                                            bucket into your EC2 server.
chmod +x ./install
sudo ./install auto                   #Runs the installer script and installs the CodeDeploy agent.
sudo service codedeploy-agent start   #Starts the installed CodeDeploy agent
sudo service codedeploy-agent status   #confirms if it is active

STEP 4 — Create IAM Role for EC2
Go to IAM
Click Roles → Create Role
Choose AWS Service → EC2

Attach policies:
AmazonS3FullAccess
AWSCodeDeployRole
Name it: EC2-CodeDeploy-Role #ggive whatever name you want
Save

Now attach this role to your EC2 instance:
EC2 → Instance → Actions → Security → Modify IAM role → Select the role


STEP 5 — Create CodeBuild Project
Open AWS Console → Search CodeBuild
Click Create build project
Name: aws-cicd-demo-build
Source: GitHub → Connect your repo

Environment:
OS: Amazon Linux 2
Runtime: Standard
Buildspec: choose Insert build commands
