# asd
sudo apt update && sudo apt install -y git
Configure User Identity
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"

git --version
mkdir git-practical-demo && cd git-practical-demo
git init
echo "DevOps Lab - Initial File Content" > app.txt
git add app.txt
git commit -m "Initial commit on main branch"
echo "Appending update for difference check" >> app.txt
git diff
git branch feature-auth

Switch branch
git checkout feature-auth

echo "Feature authentication logic implemented" > auth.txt
git add auth.txt
git commit -m "Added authentication logic in feature branch"
git checkout main
git merge feature-auth
git remote add origin https://github.com/<your-username>/git-practical-demo.git
git branch -M main
git push -u origin main
git pull origin main

Theory:
git init: Initializes a new Git repository.
git add: Adds files to the staging area.
git commit: Records staged changes in the repository history.
git push: Uploads local commits to a remote repository.
git pull: Retrieves and integrates changes from a remote repository.
git branch: Creates or displays branches in a repository.
git merge: Combines changes from one branch into another.
git diff: Displays differences between file versions or repository states.
git status: Displays the current state of the working directory and staging area.
git log: Displays the commit history.
Tracking changes in source code.
Maintaining different versions of a project.
Collaborating with multiple developers.
Creating and merging branches.
Recovering previous versions of files.
Hosting and sharing projects through GitHub.




# DynamoDB registration form
import json
import boto3
dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table('kelkar')

def lambda_handler(event, context):
    user_id = event.get('userID')
    name = event.get('name')
    email = event.get('email')
    phone = event.get('phone')
    table.put_item(
        Item={
            'userID': user_id,
            'name': name,
            'email': email,
            'phone': phone
        }
    )
    return {
        'statusCode': 200,
        'headers': {'Access-Control-Allow-Origin': '*'},
        'body': json.dumps({'message': 'Registration data stored successfully', 'userID': user_id})
    }


{
  "userId": "REG-1001",
  "name": "Alex Johnson",
  "email": "alex.johnson@example.com",
  "phone": "9876543210"
}


Theory:
Amazon DynamoDB: Stores registration details such as user ID, name, email, and phone number.
AWS Lambda: Executes code in response to events without requiring server management.
Amazon DynamoDB is a fully managed NoSQL database service provided by Amazon Web Services (AWS). It stores data in the form of key-value pairs and documents. It provides fast and flexible data access without requiring database server management.



# dynamoDB date and timelog

import boto3
from datetime import datetime

dynamodb = boto3.resource('dynamodb', region_name='ap-south-1')
table = dynamodb.Table('DateTimeTable')

def lambda_handler(event, context):
    current_time = datetime.now().isoformat()
    table.put_item(
        Item={
            'id': datetime.now().strftime('%Y%m%d%H%M%S%f'),
            'datetime': current_time
        }
    )
    return {
        'statusCode': 200,
        'body': 'Date and time stored successfully: ' + current_time
    }
User registration systems.
E-commerce applications.
Student information systems.
Attendance management systems.
Real-time applications and IoT systems.



# docker
index.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>TechVault - Home</title>
  <style>
    body { font-family: sans-serif; margin: 40px; background-color: #f7f9fc; }
    header { background: #1f2937; color: white; padding: 20px; border-radius: 8px; }
    a { color: #2563eb; font-weight: bold; text-decoration: none; }
    .btn { display: inline-block; padding: 10px 20px; background: #2563eb; color: white; border-radius: 5px; margin-top: 15px; }
  </style>
</head>
<body>
  <header>
    <h1>TechVault Store</h1>
    <p>Welcome to our modern gadget store.</p>
  </header>
  <main>
    <h2>Featured Collection</h2>
    <p>Explore high-performance peripherals and accessories.</p>
    <a class="btn" href="products.html">Browse Products &rarr;</a>
  </main>
</body>
</html>


products.html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>TechVault - Products</title>
  <style>
    body { font-family: sans-serif; margin: 40px; background-color: #f7f9fc; }
    header { background: #1f2937; color: white; padding: 20px; border-radius: 8px; }
    .card { background: white; padding: 15px; margin: 15px 0; border-radius: 8px; border: 1px solid #e5e7eb; width: 300px; }
    a { color: #2563eb; font-weight: bold; text-decoration: none; }
  </style>
</head>
<body>
  <header>
    <h1>Product Catalog</h1>
  </header>
  <main>
    <div class="card">
      <h3>Wireless Mechanical Keyboard</h3>
      <p>Price: $119</p>
    </div>
    <div class="card">
      <h3>Precision Gaming Mouse</h3>
      <p>Price: $59</p>
    </div>
    <a href="index.html">&larr; Return to Home</a>
  </main>
</body>
</html>


dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
COPY products.html /usr/share/nginx/html/products.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]


cmd commonds
dir
docker build -t ecommerce-store:v1 .
docker run -d -p 8085:80 --name ecommerce-app ecommerce-store:v1


theory:
Docker is an open-source platform used to develop, package, and run applications inside containers. Containers include the application code, libraries, dependencies, and configuration required to run an application.
Docker helps applications run consistently across different environments, such as a developer's computer, testing system, and production server.
Web application deployment.
Software development and testing.
Microservices architecture.
Continuous Integration and Continuous Deployment (CI/CD).
Cloud application development.



# jira aws git
create epic story in jira
copy link address of epic paste it in git while commiting changes
also create commit for same

theory:
Jira is a project management tool used to plan, track, and manage software development tasks. It allows users to create issues, assign tasks to team members, track progress, and manage bugs.
GitHub is a cloud-based platform used to store and manage source code using Git, a version control system.
Jira can be integrated with GitHub to connect development tasks with code changes. Developers can mention Jira issue IDs in commit messages and pull requests to link their code changes with the corresponding tasks.
AWS stands for Amazon Web Services. It is a cloud computing platform that provides services such as computing power, storage, databases, networking, and application deployment.
AWS allows developers to build, test, store, and deploy applications using cloud infrastructure.



# jenkins remote git 5 min
jenkinsfile code 
pipeline>poll scm> H/5**** > script from scm > git > url > branch > save > build now

theory:
Jenkins is an open-source automation server used to automate the software development process. It helps developers build, test, and deploy applications automatically whenever changes are made to the source code.
Jenkins integrates with version control systems such as Git and GitHub. It can automatically retrieve source code, build the application, execute tests, and deploy the application using a pipeline.
Jenkins supports plugins that allow integration with various development tools, including Git, Maven, Docker, and AWS.
Reduces manual effort. 
Detects errors early in development.
Improves software quality.
Speeds up the software delivery process.
Supports integration with multiple tools.
Provides build logs and reports for troubleshooting.


# terraform
 sudo apt update && sudo apt install -y awscli
 aws --version
 sudo dpkg --configure -a
 sudo apt update
 aws --version
 sudo apt install -y awscli
  aws --version
  aws configure
  aws sts get-caller-identity
  mkdir -p ~/terraform-ec2 && cd ~/terraform-ec2
  nano main.tf


  terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}
provider "aws" {
  region = "eu-north-1"
}
data "aws_ami" "ubuntu" {
  most_recent = true
  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-jammy-22.04-amd64-server-*"]
  }
  filter {
    name   = "virtualization-type"
    values = ["hvm"]
  }
  owners = ["099720109477"] # Canonical
}
resource "aws_instance" "devops_server" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"
  tags = {
    Name        = "Terraform-DevOps-Instance"
    Environment = "Lab-Practical-7"
  }
}
output "instance_id" {
  description = "EC2 Instance ID"
  value       = aws_instance.devops_server.id
}
output "instance_public_ip" {
  description = "Public IP Address"
  value       = aws_instance.devops_server.public_ip
}

terraform init
terraform plan
terraform apply -auto-approve


theory:
Terraform is an open-source Infrastructure as Code (IaC) tool developed by HashiCorp. It allows users to create, modify, and manage cloud infrastructure using configuration files instead of manually creating resources through a cloud console.
Terraform supports multiple cloud providers, including Amazon Web Services (AWS), Microsoft Azure, and Google Cloud Platform (GCP).
terraform init: Initializes the Terraform project.
terraform validate: Validates configuration files.
terraform plan: Previews proposed changes.
terraform apply: Applies the configuration to create or modify resources.
terraform destroy: Destroys managed infrastructure resources.
terraform fmt: Formats configuration files.
terraform show: Displays the current state or a saved plan.



# airflow
Automates repetitive workflows.
Supports scheduled task execution.
Provides a graphical interface for monitoring.
Supports task dependencies and retries.
Uses Python for workflow development.
Helps manage complex data pipelines.

### Requirements
- Docker Desktop installed and running on Windows, with WSL integration enabled if using Ubuntu/WSL
- Docker available in the terminal

 Steps

1. Check Docker:
   bash
   docker --version
   docker run --rm hello-world
2. Create the Airflow DAG folder:
   bash
   mkdir -p ~/practical9_airflow/dags
   cd ~/practical9_airflow
3. Create the Python DAG file:
   bash
   nano dags/datetime_wednesday_dag.py
4. Paste the code:

   ```python
   from datetime import datetime, timedelta

   from airflow import DAG
   from airflow.operators.python import PythonOperator


   def show_datetime():
       print("Current Date and Time:", datetime.now())


   default_args = {
       "owner": "airflow",
       "retries": 1,
       "retry_delay": timedelta(minutes=5),
   }


   with DAG(
       dag_id="wednesday_datetime_display",
       default_args=default_args,
       start_date=datetime(2026, 1, 1),
       schedule="30 9 * * 3",
       catchup=False,
   ) as dag:
       task = PythonOperator(
           task_id="display_datetime_task",
           python_callable=show_datetime,
       )
   ```

5. Save the file and start Airflow in Docker:
   bash
   docker run -d \
   name airflow-standalone \
     -p 8085:8080 \
     -v "${PWD}/dags:/opt/airflow/dags" \
     apache/airflow:2.9.2 \
     airflow standalone
6. Get the generated login credentials from the container logs:
   ```bash
   docker logs airflow-standalone
   ```
7. Open this URL in your browser:
   `http://localhost:8085`
   Sign in using the credentials shown in the logs. Find `wednesday_datetime_display`, enable it if needed, and trigger it manually.
8. To test the task from the terminal:
   ```bash
   docker exec -it airflow-standalone \
     airflow tasks test \
     wednesday_datetime_display \
     display_datetime_task \
     2026-10-28
   ```
**Expected result:** The task runs and logs the current date and time.

**Schedule:** `30 9 * * 3` means every Wednesday at 9:30 AM, using the Airflow environment's configured timezone.


## Practical 2: Ansible — Localhost Configuration Automation

### Requirements
- Windows Subsystem for Linux (WSL) with Ubuntu, or an Ubuntu system
- Internet connection for package installation

### Steps
1. In Windows PowerShell, check WSL:
   ```powershell
   wsl --status
   wsl --list --verbose
   ```
2. If Ubuntu is not installed, run this in PowerShell:
   ```powershell
   wsl --install -d Ubuntu
   ``
   Restart if prompted, then open Ubuntu and create a Linux username and password.
3. In the Ubuntu terminal, install Ansible:
   ```bash
   sudo apt update
   sudo apt install -y ansible
   ansible --version
   ```
4. Create the working folder and inventory:
   ```bash
   mkdir -p ~/ansible_practical5
   cd ~/ansible_practical5
   cat << 'EOF' > inventory.ini
   [local]
   localhost ansible_connection=local
   EOF
   ```
5. Create the playbook:
   ```bash
   nano playbook.yml
   ```
6. Paste the following YAML code. Keep the indentation exactly as shown.

   ```yaml
   ---
   - name: Ansible Localhost Configuration Automation
     hosts: localhost
     connection: local
     gather_facts: yes

     tasks:
       - name: Display system information
         ansible.builtin.debug:
           msg: "OS is {{ ansible_distribution }} {{ ansible_distribution_version }} on {{ ansible_architecture }}"

       - name: Create deployment directory
         ansible.builtin.file:
           path: /tmp/ansible_deployment
           state: directory
           mode: '0755'

       - name: Create deployment metadata file
         ansible.builtin.copy:
           dest: /tmp/ansible_deployment/deploy_meta.txt
           content: |
             Environment=Production
             Provisioned_Date={{ ansible_date_time.date }}
             Provisioned_Time={{ ansible_date_time.time }}
             Orchestrator=Ansible_Automation
           mode: '0644'

       - name: Read deployment metadata
         ansible.builtin.command:
           cmd: cat /tmp/ansible_deployment/deploy_meta.txt
         register: result
         changed_when: false

       - name: Display metadata
         ansible.builtin.debug:
           var: result.stdout_lines
   ```

   Save using `Ctrl+O`, press Enter, and exit using `Ctrl+X`.
7. Check syntax and run the playbook:
   ```bash
   ansible-playbook --syntax-check -i inventory.ini playbook.yml
   ansible-playbook -i inventory.ini playbook.yml
8. Verify the file:
   ```bash
   cat /tmp/ansible_deployment/deploy_meta.txt
   ```
**Expected result:** Ansible gathers system facts, creates the deployment directory and metadata file, and displays the file content. The play recap should show `failed=0`.





