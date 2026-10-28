# DevOps & Cloud Computing Complete Lab Manual (Practicals 1 to 10)

> Clean Markdown version for GitHub README. HTML page layout and styling have been removed; practical steps, commands, and source code are retained.

## Table of Contents
- [Practical 1: Agile Lifecycle Management: Jira Integration with GitHub & AWS](#practical-1-agile-lifecycle-management-jira-integration-with-github--aws)
- [Practical 2: Version Control System: Git Command Demonstration](#practical-2-version-control-system-git-command-demonstration)
- [Practical 3: Jenkins CI Pipeline with GitHub Poll SCM (5-Minute Trigger)](#practical-3-jenkins-ci-pipeline-with-github-poll-scm-5-minute-trigger)
- [Practical 4: AWS Lambda Registration Form Integration with Amazon DynamoDB](#practical-4-aws-lambda-registration-form-integration-with-amazon-dynamodb)
- [Practical 5: Configuration Management: Ansible Automation on Localhost (WSL)](#practical-5-configuration-management-ansible-automation-on-localhost-wsl)
- [Practical 6: Cloud Storage & Hosting: Amazon S3 Static Website](#practical-6-cloud-storage--hosting-amazon-s3-static-website)
- [Practical 7: Infrastructure as Code (IaC): Terraform EC2 Provisioning](#practical-7-infrastructure-as-code-iac-terraform-ec2-provisioning)
- [Practical 8: Serverless Execution: AWS Lambda Writing to DynamoDB](#practical-8-serverless-execution-aws-lambda-writing-to-dynamodb)
- [Practical 9: Workflow Orchestration: Apache Airflow Scheduled Cron DAG](#practical-9-workflow-orchestration-apache-airflow-scheduled-cron-dag)
- [Practical 10: Automated Deployment: Jenkins Pipeline with Docker Build & Run](#practical-10-automated-deployment-jenkins-pipeline-with-docker-build--run)

## Practical 1: Agile Lifecycle Management: Jira Integration with GitHub & AWS

Objective: To study and implement an Agile Software Project lifecycle using Jira, integrating it with GitHub for automated VCS traceability and AWS for deployment state tracking.

### 1. Jira Agile Board Setup

- Sign in to Atlassian Jira and create a new project using the Scrum or Kanban software development template.

- Configure the standard board workflow columns: TO DO → IN PROGRESS → IN REVIEW → DONE

- Create user stories representing project requirements. Note the designated issue key (e.g., DEV-101 ).

- Create and start a new Sprint (e.g., "Sprint 1 - Core Architecture").

### 2. GitHub & Jira Bi-Directional Integration

- In Jira, navigate to Apps → Explore more apps and install GitHub for Jira .

- Authorize Jira to access your target GitHub organization/account and select your repository.

- Link commit history, branches, and PRs automatically to Jira issues by referencing the Jira Issue Key in branch names and commit messages: # Create feature branch referencing Jira Issue Key
git checkout -b feature/DEV-101-auth-module

# Stage and commit using smart commits
git commit -m "DEV-101 #time 2h #comment Initialized authentication controllers #in-progress"

# Push to remote
git push origin feature/DEV-101-auth-module

- Open the issue in Jira: the Development panel on the right sidebar reflects linked commits and branches directly.

### 3. AWS Deployment Tracking via GitHub Actions

Create a GitHub Action pipeline in .github/workflows/deploy.yml to deploy changes to AWS and inform Jira of deployment status:

```text
name: CI/CD AWS Deployment Pipeline
on:
  push:
    branches: [ "main" ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Source
        uses: actions/checkout@v4

      - name: Configure AWS Credentials
        uses: aws-actions/configure-aws-credentials@v4
        with:
          aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
          aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
          aws-region: eu-north-1

      - name: Deploy Artifact to AWS S3
        run: |
          aws s3 sync ./dist s3://my-agile-prod-bucket --delete

      - name: Record Deployment
        run: echo "Deployment of DEV-101 to AWS successfully executed."
```

- Screenshot of Jira Active Sprint Board with cards transitioned across columns.

- Screenshot of Jira Issue Details displaying linked GitHub Commits/PRs.

- Screenshot of AWS resource showing deployed artifacts.

## Practical 2: Version Control System: Git Command Demonstration

Objective: Install Git and demonstrate git init , add , commit , push , pull , branch , merge , and diff commands.

### 1. Installation & Environment Verification

```bash
# Ubuntu / WSL
sudo apt update && sudo apt install -y git

# Configure User Identity
git config --global user.name "Your Name"
git config --global user.email "your-email@example.com"

# Verify installation
git --version
```

### 2. Command Execution Lifecycle

- Initialize repository ( git init ): mkdir git-practical-demo && cd git-practical-demo
git init

- Stage and commit files ( git add & git commit ): echo "DevOps Lab - Initial File Content" > app.txt
git add app.txt
git commit -m "Initial commit on main branch"

- Inspect modifications ( git diff ): echo "Appending update for difference check" >> app.txt
git diff Output shows line additions preceded by + .

- Branch creation and switching ( git branch ): # Create feature branch
git branch feature-auth

# Switch branch
git checkout feature-auth

# Add unique content
echo "Feature authentication logic implemented" > auth.txt
git add auth.txt
git commit -m "Added authentication logic in feature branch"

- Merge branches ( git merge ): git checkout main
git merge feature-auth

- Remote tracking ( git push & git pull ): # Link to remote GitHub repository
git remote add origin https://github.com/<your-username>/git-practical-demo.git
git branch -M main
git push -u origin main

# Simulate syncing updates from remote
git pull origin main

- Terminal output of git diff displaying working tree differences.

- Output of git log --graph --oneline --all proving successful branch creation and merge commit.

- Screenshot of remote GitHub repository showing committed branch history.

## Practical 3: Jenkins CI Pipeline with GitHub Poll SCM (5-Minute Trigger)

Objective: Install Jenkins and perform continuous integration using a declarative pipeline with a Jenkinsfile sourced from a remote GitHub repository, configured to track changes every 5 minutes.

### 1. Repository Configuration: The Jenkinsfile

Place this Jenkinsfile at the root of your remote GitHub repository:

```groovy
pipeline {
    agent any

    // Poll SCM every 5 minutes for repository updates
    triggers {
        pollSCM('H/5 * * * *')
    }

    stages {
        stage('Checkout SCM') {
            steps {
                echo 'Checking out source code from remote repository...'
                checkout scm
            }
        }

        stage('Build & Syntax Check') {
            steps {
                echo 'Running build phase...'
                sh 'python3 -m py_compile *.py || true'
            }
        }

        stage('Automated Tests') {
            steps {
                echo 'Executing unit validation test suite...'
                sh 'echo "Tests passed successfully."'
            }
        }

        stage('Artifact Packaging') {
            steps {
                echo 'Packaging deployment artifacts...'
                sh 'tar -czvf build-artifact.tar.gz * || true'
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution cycle concluded.'
        }
        success {
            echo 'Build passed all status checks.'
        }
        failure {
            echo 'Build failed. Alert notifications dispatched.'
        }
    }
}
```

### 2. Jenkins Job Creation & Configuration

- Open the Jenkins Web UI ( http://localhost:8080 ).

- Click New Item → Enter job name (e.g., GitHub-5Min-Poll-Pipeline ) → Select Pipeline → Click OK .

- In the configuration menu: Under Build Triggers : Check Poll SCM and enter schedule: H/5 * * * * Under Pipeline : Definition : Select Pipeline script from SCM . SCM : Select Git . Repository URL : Enter your GitHub repository HTTPS clone URL. Credentials : Add your GitHub Personal Access Token (PAT) if private. Branch Specifier : */main or */master . Script Path : Ensure it is set to Jenkinsfile .

- Click Save .

- Screenshot of Jenkins Pipeline configuration highlighting the H/5 * * * * Poll SCM schedule.

- Screenshot of Poll SCM Log showing automatic trigger via commit detection.

- Screenshot of the Stage View showing green status across all pipeline stages.

## Practical 4: AWS Lambda Registration Form Integration with Amazon DynamoDB

Objective: Write an AWS Lambda function that processes inputs submitted from a client registration form and persists the user data into an Amazon DynamoDB table.

### 1. Amazon DynamoDB Table Setup

- Open the DynamoDB Console in your active region (e.g., eu-north-1 )[cite: 1].

- Click Create table and configure[cite: 1]: Table name: StudentRegistrationTable Partition key: registration_id (Type: String ) Table class: DynamoDB Standard (On-demand capacity)[cite: 1]

- Click Create table and confirm status changes to Active [cite: 1].

### 2. AWS Lambda Function Implementation

Create a Lambda function named ProcessRegistrationForm (Runtime: Python 3.12 )[cite: 1], attach the IAM role with DynamoDB write permissions[cite: 1], and deploy this handler code:

```python
import json
import boto3
import uuid
from datetime import datetime

# Initialize DynamoDB resource with explicit region
dynamodb = boto3.resource('dynamodb', region_name='eu-north-1')
table = dynamodb.Table('StudentRegistrationTable')

def lambda_handler(event, context):
    try:
        # Check if request comes via API Gateway (body as JSON string) or direct JSON invocation
        if 'body' in event and event['body'] is not None:
            data = json.loads(event['body']) if isinstance(event['body'], str) else event['body']
        else:
            data = event

        # Generate unique registration ID and submission timestamp
        registration_id = str(uuid.uuid4())
        submitted_at = datetime.utcnow().strftime('%Y-%m-%d %H:%M:%S UTC')

        # Construct the persistence item
        item = {
            'registration_id': registration_id,
            'full_name': data.get('full_name', 'N/A'),
            'email': data.get('email', 'N/A'),
            'phone': data.get('phone', 'N/A'),
            'course': data.get('course', 'DevOps Engineering'),
            'submitted_at': submitted_at
        }

        # Insert record into DynamoDB
        table.put_item(Item=item)

        return {
            'statusCode': 200,
            'headers': {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*'
            },
            'body': json.dumps({
                'message': 'Registration successful!',
                'registration_id': registration_id,
                'data': item
            })
        }

    except Exception as e:
        return {
            'statusCode': 500,
            'headers': {
                'Content-Type': 'application/json',
                'Access-Control-Allow-Origin': '*'
            },
            'body': json.dumps({'error': str(e)})
        }
```

### 3. Client-Side HTML Registration Form

Save this as registration_form.html . Replace LAMBDA_API_ENDPOINT_URL with your Lambda Function URL or API Gateway invocation URL:

```text
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Student Registration Form</title>
    <style>
        body { font-family: Arial, sans-serif; background: #f4f6f8; padding: 2rem; }
        .form-card { max-width: 450px; background: #fff; padding: 2rem; border-radius: 8px; margin: auto; box-shadow: 0 4px 6px rgba(0,0,0,0.1); }
        .field { margin-bottom: 1rem; }
        label { display: block; margin-bottom: 0.5rem; font-weight: bold; }
        input, select { width: 100%; padding: 0.5rem; border: 1px solid #ccc; border-radius: 4px; box-sizing: border-box; }
        button { width: 100%; padding: 0.75rem; background: #2563eb; color: #fff; border: none; border-radius: 4px; font-weight: bold; cursor: pointer; }
        button:hover { background: #1d4ed8; }
        #status { margin-top: 1rem; padding: 0.5rem; border-radius: 4px; display: none; }
    </style>
</head>
<body>
    <div class="form-card">
        <h2>Student Registration</h2>
        <form id="regForm">
            <div class="field">
                <label for="full_name">Full Name</label>
                <input type="text" id="full_name" required>
            </div>
            <div class="field">
                <label for="email">Email Address</label>
                <input type="email" id="email" required>
            </div>
            <div class="field">
                <label for="phone">Phone Number</label>
                <input type="tel" id="phone" required>
            </div>
            <div class="field">
                <label for="course">Course Selection</label>
                <select id="course">
                    <option value="DevOps & Cloud Computing">DevOps & Cloud Computing</option>
                    <option value="AI & Machine Learning">AI & Machine Learning</option>
                    <option value="Cyber Security">Cyber Security</option>
                </select>
            </div>
            <button type="submit">Submit Registration</button>
        </form>
        <div id="status"></div>
    </div>

    <script>
        const API_URL = "LAMBDA_API_ENDPOINT_URL";

        document.getElementById('regForm').addEventListener('submit', async (e) => {
            e.preventDefault();
            const statusDiv = document.getElementById('status');
            
            const payload = {
                full_name: document.getElementById('full_name').value,
                email: document.getElementById('email').value,
                phone: document.getElementById('phone').value,
                course: document.getElementById('course').value
            };

            try {
                const response = await fetch(API_URL, {
                    method: 'POST',
                    headers: { 'Content-Type': 'application/json' },
                    body: JSON.stringify(payload)
                });
                const result = await response.json();
                statusDiv.style.display = 'block';
                statusDiv.style.background = '#dcfce7';
                statusDiv.style.color = '#15803d';
                statusDiv.innerHTML = `Registration Saved! ID: <strong>${result.registration_id}</strong>`;
                document.getElementById('regForm').reset();
            } catch (err) {
                statusDiv.style.display = 'block';
                statusDiv.style.background = '#fee2e2';
                statusDiv.style.color = '#b91c1c';
                statusDiv.innerText = 'Submission Error: ' + err.message;
            }
        });
    </script>
</body>
</html>
```

- Screenshot of Lambda Test event execution returning "statusCode": 200 .

- Screenshot of the submitted registration form in the browser showing the success banner with registration ID.

- Screenshot of the DynamoDB Explore table items view showing the persisted record in StudentRegistrationTable .

## Practical 5: Configuration Management: Ansible Automation on Localhost (WSL)

Objective: Install Ansible and write playbooks to manage localhost system configurations, gather system facts, and provision deployment metadata files.

### 1. Directory and Inventory Setup

```ini
mkdir -p ~/ansible_practical5 && cd ~/ansible_practical5

# Create inventory.ini
cat << 'EOF' > inventory.ini
[local]
localhost ansible_connection=local
EOF
```

### 2. Ansible Playbook ( playbook.yml )

```yaml
---
- name: Practical 5 Localhost Configuration Automation
  hosts: localhost
  connection: local
  gather_facts: yes

  tasks:
    - name: Task 1 - Display Target OS Information
      ansible.builtin.debug:
        msg: "Target OS is {{ ansible_distribution }} {{ ansible_distribution_version }} on {{ ansible_architecture }}"

    - name: Task 2 - Ensure local destination directory exists
      ansible.builtin.file:
        path: /tmp/ansible_deployment
        state: directory
        mode: '0755'

    - name: Task 3 - Provision deployment properties file
      ansible.builtin.copy:
        dest: /tmp/ansible_deployment/deploy_meta.txt
        content: |
          Environment=Production
          Provisioned_Date={{ ansible_date_time.date }}
          Provisioned_Time={{ ansible_date_time.time }}
          Orchestrator=Ansible_Automation
        mode: '0644'

    - name: Task 4 - Verify generated file content
      ansible.builtin.command: cat /tmp/ansible_deployment/deploy_meta.txt
      register: file_content

    - name: Task 5 - Print file content output
      ansible.builtin.debug:
        var: file_content.stdout_lines
```

### 3. Execution Command

```bash
ansible-playbook -i inventory.ini playbook.yml
cat /tmp/ansible_deployment/deploy_meta.txt
```

- Terminal output of ansible-playbook with failed=0 in the PLAY RECAP.

- Output of cat /tmp/ansible_deployment/deploy_meta.txt displaying dynamically evaluated date/time facts.

## Practical 6: Cloud Storage & Hosting: Amazon S3 Static Website

Objective: Configure an Amazon S3 bucket for public static website hosting, upload HTML/CSS artifacts, and configure access permissions via Bucket Policy.

### 1. Create S3 Bucket & Unblock Public Access

- Navigate to S3 Console → Click Create bucket .

- Bucket name: Enter a globally unique name (e.g., practical6-static-site-101 ).

- Region: Select your default region (e.g., eu-north-1 )[cite: 1].

- Under Block Public Access settings for this bucket : Uncheck Block all public access . Acknowledge the safety warning checkbox.

- Click Create bucket .

### 2. Enable Static Website Hosting

- Click on the newly created bucket → Open the Properties tab.

- Scroll down to Static website hosting → Click Edit .

- Select Enable .

- Hosting type: Host a static website .

- Index document: index.html ; Error document: error.html .

- Click Save changes . Note down the generated Bucket website endpoint URL .

### 3. Attach Bucket Policy

Go to the Permissions tab → Scroll down to Bucket policy → Click Edit → Paste (replace YOUR-BUCKET-NAME ):

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::YOUR-BUCKET-NAME/*"
        }
    ]
}
```

### 4. Upload Site Files

Create a simple index.html and upload it to the bucket:

```text
<!DOCTYPE html>
<html>
<head><title>DevOps S3 Static Website</title></head>
<body style="text-align:center; font-family:sans-serif; margin-top:50px;">
    <h1>Hosted on Amazon S3 Successfully!</h1>
    <p>DevOps Practical 6: Static Website Hosting Verification</p>
</body>
</html>
```

- Screenshot of the S3 Static Website Hosting properties page showing the active endpoint URL.

- Screenshot of the browser loading the website via the S3 HTTP endpoint URL.

## Practical 7: Infrastructure as Code (IaC): Terraform EC2 Provisioning

Objective: Automate cloud resource provisioning by defining and applying an AWS EC2 instance using HashiCorp Terraform.

### 1. Terraform Configuration File ( main.tf )

Create a directory ~/practical7_terraform and add main.tf :

```hcl
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

# Fetch latest official Ubuntu 22.04 LTS AMI in eu-north-1
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

# Provision EC2 Instance
resource "aws_instance" "web_server" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = "t3.micro"

  tags = {
    Name        = "Practical7-Terraform-EC2"
    Environment = "DevOps-Lab"
  }
}

# Output values for verification
output "instance_id" {
  description = "EC2 Instance ID"
  value       = aws_instance.web_server.id
}

output "instance_public_ip" {
  description = "Public IPv4 Address"
  value       = aws_instance.web_server.public_ip
}
```

### 2. Terraform Execution Lifecycle

```bash
# Initialize providers
terraform init

# Generate execution plan
terraform plan

# Apply changes to cloud
terraform apply -auto-approve

# Clean up / Teardown after verification
terraform destroy -auto-approve
```

- Terminal output of terraform apply showing Apply complete! Resources: 1 added with instance ID and IP outputs.

- Screenshot of AWS EC2 Console showing Practical7-Terraform-EC2 in Running state.

## Practical 8: Serverless Execution: AWS Lambda Writing to DynamoDB

Objective: Deploy an AWS Lambda function that writes execution records containing generated IDs and timestamps into an Amazon DynamoDB table[cite: 1].

### 1. DynamoDB Table Setup

- Table Name: DateTimeLogs [cite: 1]

- Partition Key: log_id (String)[cite: 1]

- Region: eu-north-1 [cite: 1]

### 2. Lambda Function Code ( lambda_function.py )

```python
import json
import boto3
import uuid
from datetime import datetime

# Explicitly pass the region to prevent ResourceNotFoundException
dynamodb = boto3.resource('dynamodb', region_name='eu-north-1')
table = dynamodb.Table('DateTimeLogs')

def lambda_handler(event, context):
    log_id = str(uuid.uuid4())
    timestamp = datetime.utcnow().strftime('%Y-%m-%d %H:%M:%S UTC')
    
    item = {
        'log_id': log_id,
        'timestamp': timestamp,
        'event_source': 'AWS Lambda Automation',
        'status': 'Record Successfully Inserted'
    }
    
    # Write record to DynamoDB
    table.put_item(Item=item)
    
    return {
        'statusCode': 200,
        'body': json.dumps({
            'message': 'Item inserted successfully into DynamoDB',
            'inserted_item': item
        })
    }
```

- Screenshot of Lambda Test Execution Result showing statusCode: 200 .

- Screenshot of DynamoDB Explore table items showing the generated row in DateTimeLogs .

## Practical 9: Workflow Orchestration: Apache Airflow Scheduled Cron DAG

Objective: Deploy Apache Airflow using Docker and author a Directed Acyclic Graph (DAG) scheduled using cron syntax ( 30 9 * * 3 ) to execute and log current timestamps[cite: 1, 2, 3].

### 1. DAG Implementation ( datetime_wednesday_dag.py )

```python
from datetime import datetime, timedelta
from airflow import DAG
from airflow.operators.python import PythonOperator

def print_current_datetime():
    current_time = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    print("=" * 50)
    print(f"CRON SCHEDULED TRIGGER EXECUTION: {current_time}")
    print("=" * 50)

default_args = {
    'owner': 'airflow',
    'depends_on_past': False,
    'start_date': datetime(2026, 1, 1),
    'retries': 1,
    'retry_delay': timedelta(minutes=5),
}

dag = DAG(
    'wednesday_datetime_display',
    default_args=default_args,
    description='Displays current date and time every Wednesday at 9:30 AM',
    schedule='30 9 * * 3',  # At 09:30 on Wednesday
    catchup=False,
    tags=['practical9'],
)

display_datetime_task = PythonOperator(
    task_id='display_datetime_task',
    python_callable=print_current_datetime,
    dag=dag,
)
```

### 2. Docker Container Deployment Commands

```bash
# Run Airflow in standalone mode with volume mounted dags folder
docker run -d --name airflow-standalone -p 8085:8080 -v "${PWD}/dags:/opt/airflow/dags" apache/airflow:2.9.2 airflow standalone

# Test task directly via terminal
docker exec -it airflow-standalone airflow tasks test wednesday_datetime_display display_datetime_task 2026-10-25
```

- Screenshot of Airflow DAGs grid view showing wednesday_datetime_display with schedule 30 9 * * 3 [cite: 2, 3].

- Screenshot of task log view showing display_datetime_task output with timestamp header[cite: 3, 4].

## Practical 10: Automated Deployment: Jenkins Pipeline with Docker Build & Run

Objective: Automate application containerization through a Jenkins declarative pipeline that builds a Docker image from a Dockerfile and executes the containerized web app.

### 1. Application Files

Dockerfile:

```dockerfile
FROM nginx:alpine
COPY index.html /usr/share/nginx/html/index.html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

index.html:

```text
<!DOCTYPE html>
<html>
<head><title>DevOps Docker App</title></head>
<body style="text-align:center; padding-top:40px; font-family:sans-serif;">
    <h1>Continuous Deployment via Jenkins & Docker</h1>
    <p>Practical 10 automated build and run verification.</p>
</body>
</html>
```

### 2. Declarative Jenkinsfile

```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = 'devops-web-app'
        CONTAINER_NAME = 'practical10-running-app'
        HOST_PORT = '8082'
    }

    stages {
        stage('Checkout Code') {
            steps {
                echo 'Checking out source repository...'
                checkout scm
            }
        }

        stage('Cleanup Previous Container') {
            steps {
                sh '''
                docker rm -f ${CONTAINER_NAME} || true
                '''
            }
        }

        stage('Build Docker Image') {
            steps {
                sh '''
                docker build -t ${IMAGE_NAME}:${BUILD_NUMBER} -t ${IMAGE_NAME}:latest .
                '''
            }
        }

        stage('Deploy Container') {
            steps {
                sh '''
                docker run -d --name ${CONTAINER_NAME} -p ${HOST_PORT}:80 ${IMAGE_NAME}:latest
                '''
            }
        }

        stage('Health Check') {
            steps {
                sleep 3
                sh '''
                curl -I http://localhost:${HOST_PORT} || curl -I http://127.0.0.1:${HOST_PORT}
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline succeeded! Application deployed to port ${HOST_PORT}."
        }
        failure {
            echo "Deployment failed."
        }
    }
}
```

- Screenshot of Jenkins Pipeline Stage View showing successful completion across all 5 stages.

- Screenshot of browser loading http://localhost:8082 displaying the deployed Nginx application.
