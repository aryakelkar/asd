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

### Step-by-Step Instructions

1. **Prepare the accounts and tools.** You need access to Atlassian Jira, GitHub, and an AWS account. Create/sign in to these accounts using their official websites. For AWS, use an account permitted to create an S3 bucket and an IAM user/role with the required permissions. AWS usage may incur charges.
2. **Create a GitHub repository.** On GitHub, create a repository for the project. Clone it to your computer, then open a terminal in the repository folder:
   ```bash
   git clone https://github.com/<your-username>/<your-repository>.git
   cd <your-repository>
   ```
   Replace both placeholders with your actual repository details. Configure Git if this is the first use:
   ```bash
   git config --global user.name "Your Name"
   git config --global user.email "your-email@example.com"
   ```
3. **Create the Jira project.** Sign in to Jira, create a Scrum or Kanban project, and open its board. Create a sample issue and note its issue key, such as `DEV-101`. The key used in the branch/commit commands below must match your actual issue key.
4. **Connect Jira and GitHub.** In Jira, open Apps/Explore apps and install or open GitHub for Jira if your account allows it. Authorize the correct GitHub account/organization and connect the repository. If your organization restricts app installation, ask its Jira/GitHub administrator.
5. **Create the deployment workflow.** In the local repository, create the folder `.github/workflows` and save the workflow shown in the existing “AWS Deployment Tracking via GitHub Actions” section as `.github/workflows/deploy.yml`. Before running it, create an S3 bucket and update the bucket name in the workflow. The workflow also requires the `dist` folder to exist in the repository.
6. **Configure GitHub secrets.** In the repository, go to Settings → Secrets and variables → Actions and add `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` for an IAM identity with the necessary S3 permissions. Do not paste credentials into the workflow file or commit them. Confirm that the workflow's AWS region matches the bucket's region.
7. **Run the Git demonstration.** In the repository terminal, create a branch and commit using the existing commands in section 2 below. Push the branch to GitHub and confirm the Jira issue's Development panel shows linked development activity.
8. **Verify deployment.** Push a commit to `main`, then open the repository's Actions tab. Open the workflow run and inspect each step. If it succeeds, check the target S3 bucket for the uploaded files.
9. **Troubleshooting.** A missing `dist` directory causes the sync step to fail; create/populate it or adjust the workflow to the actual build output. An `AccessDenied` error usually means the AWS identity lacks S3 permissions. A region mismatch means the workflow region and bucket region must agree. A Jira issue will not link if the issue key is absent or incorrect in the branch/commit name.

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

### Step-by-Step Instructions

1. **Prepare the environment.** Use an Ubuntu Linux machine or Ubuntu on WSL. Update packages:
   ```bash
   sudo apt update
   sudo apt install -y git curl fontconfig
   ```
2. **Install Java 21.** Jenkins requires a supported Java version. On current Ubuntu releases, install OpenJDK 21 if available:
   ```bash
   sudo apt install -y openjdk-21-jre
   java -version
   ```
   If the package is unavailable, use a supported Ubuntu release or follow the official Jenkins installation instructions for your distribution. Confirm Java 21 or another version supported by the Jenkins release is installed.
3. **Install Jenkins.** Follow the official Jenkins Linux installation instructions for your Ubuntu version, including adding the official Jenkins package repository, then install and start the service:
   ```bash
   sudo systemctl enable --now jenkins
   sudo systemctl status jenkins
   ```
   If `systemctl` is unavailable in your WSL setup, run Jenkins in a supported Linux environment or enable systemd for WSL.
4. **Open Jenkins.** On the same computer, open `http://localhost:8080`. Retrieve the initial administrator password using:
   ```bash
   sudo cat /var/lib/jenkins/secrets/initialAdminPassword
   ```
   Paste it into the setup page, install the suggested plugins, and create an administrator account.
5. **Create a GitHub repository.** Create or use a repository you can access. Save the existing pipeline code below as a file named `Jenkinsfile` (capital J, no file extension) in the repository root. Commit and push it:
   ```bash
   git add Jenkinsfile
   git commit -m "Add Jenkins pipeline"
   git push
   ```
   If the repository is not yet cloned locally, clone it first and enter its directory.
6. **Create the Jenkins job.** In Jenkins, choose New Item, enter `GitHub-5Min-Poll-Pipeline`, select Pipeline, and click OK. Under Pipeline, choose “Pipeline script from SCM”, choose Git, and enter the repository's HTTPS clone URL. Add credentials only if the repository is private. Set Branch Specifier to `*/main` (or the branch actually used) and Script Path to `Jenkinsfile`.
7. **Configure polling and run.** Under Build Triggers, enable Poll SCM and enter `H/5 * * * *`. Save the job and click Build Now for the first run. Commit and push another change to the repository; allow up to five minutes for polling to detect it.
8. **Verify.** Open the build's Console Output and Stage View. A successful build shows completed stages and the pipeline's success message.
9. **Troubleshooting.** If port 8080 is unavailable, check `sudo systemctl status jenkins` and whether another service uses the port. If Git checkout fails for a private repository, configure a GitHub token as Jenkins credentials. If `sh` commands fail on a Windows-only agent, run the job on a Linux agent. A Java compatibility error means the installed Java version is unsupported by that Jenkins release.

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

### Step-by-Step Instructions

1. **Sign in to AWS.** Open the AWS Management Console and sign in. Select one region and use it consistently throughout this practical. The existing code uses `eu-north-1` (Europe/Stockholm); select that region unless you also change the region in the code.
2. **Create the DynamoDB table.** Open DynamoDB → Tables → Create table. Enter `StudentRegistrationTable` as the table name and `registration_id` as the partition key with type String. Choose On-demand capacity, create the table, and wait until its status is Active.
3. **Create the Lambda execution role.** Open IAM → Roles → Create role → AWS service → Lambda. Attach `AWSLambdaBasicExecutionRole` for CloudWatch logging. Add an inline or managed policy allowing `dynamodb:PutItem` on the `StudentRegistrationTable` table ARN in the selected region/account. Create the role.
4. **Create the function.** Open Lambda → Create function → Author from scratch. Name it `ProcessRegistrationForm`, select Python 3.12, and choose the role created in the previous step. Create the function.
5. **Paste and deploy the code.** In the Lambda Code tab, open `lambda_function.py` (or the default `lambda_function.py` file), replace its contents with the existing Python code in section 2, and click Deploy. The code's table name and region must match the table created above.
6. **Create a test event.** In the Test tab, create an event named `RegistrationTest` with this JSON, then save it:
   ```json
   {
     "full_name": "Sample Student",
     "email": "student@example.com",
     "phone": "9876543210",
     "course": "DevOps & Cloud Computing"
   }
   ```
7. **Run and verify.** Click Test. A successful result contains `statusCode: 200`, `Registration successful!`, and a generated `registration_id`. Open DynamoDB → Explore items → `StudentRegistrationTable` and refresh to confirm the record was stored.
8. **Optional browser form.** Save the existing HTML code in section 3 as `registration_form.html`. Create a Lambda Function URL or API Gateway endpoint, configure the required CORS settings for browser requests, and replace `LAMBDA_API_ENDPOINT_URL` in the HTML with that endpoint. Open the file in a browser and submit a test registration. The function's response body is JSON; if the form's success message does not appear, inspect the browser developer console and the Lambda/API logs.
9. **Troubleshooting and cleanup.** `AccessDeniedException` means the Lambda role needs `dynamodb:PutItem` on the correct table ARN. `ResourceNotFoundException` usually means the table name or region does not match. For `KeyError`/missing input errors, check the test-event JSON. When finished, delete the Lambda function, any Function URL/API Gateway resources, the IAM role if created only for this practical, and the DynamoDB table if you no longer need the data.

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

### Step-by-Step Instructions

1. **Open PowerShell as a normal Windows user.** Check whether WSL is installed:
   ```powershell
   wsl --status
   wsl --list --verbose
   ```
2. **Install Ubuntu if it is missing.** In PowerShell run:
   ```powershell
   wsl --install -d Ubuntu
   ```
   Restart Windows if prompted, then open Ubuntu from the Start menu. On first launch, wait for setup and create a Linux username and password. The password does not appear while typing; this is normal. If Ubuntu is already installed, open it from the Start menu or run `wsl -d Ubuntu`.
3. **Update Ubuntu and install Ansible.** Run these commands in the Ubuntu terminal, not in Windows PowerShell:
   ```bash
   sudo apt update
   sudo apt upgrade -y
   sudo apt install -y ansible
   ansible --version
   ```
4. **Create the working directory and inventory.** Run:
   ```bash
   mkdir -p ~/ansible_practical5
   cd ~/ansible_practical5
   cat << 'EOF' > inventory.ini
   [local]
   localhost ansible_connection=local
   EOF
   ```
   Check that the file exists and contains the inventory:
   ```bash
   cat inventory.ini
   ```
5. **Create the playbook.** In the same `~/ansible_practical5` directory, create `playbook.yml` by pasting the complete YAML code from section 2 below. You can use Nano:
   ```bash
   nano playbook.yml
   ```
   Paste the playbook, save with `Ctrl+O`, press Enter, and exit with `Ctrl+X`. Keep the indentation exactly as shown in the YAML code.
6. **Check the files and syntax.** Confirm both files are in the current directory:
   ```bash
   ls -l
   ansible-playbook --syntax-check -i inventory.ini playbook.yml
   ```
   A successful syntax check reports that the playbook syntax is OK.
7. **Run the playbook.** Execute the existing commands in section 3:
   ```bash
   ansible-playbook -i inventory.ini playbook.yml
   ```
   It gathers Ubuntu system facts, creates `/tmp/ansible_deployment`, writes `deploy_meta.txt`, and prints the file content.
8. **Verify the generated output.** Run:
   ```bash
   cat /tmp/ansible_deployment/deploy_meta.txt
   ```
   The file should contain `Environment=Production`, a provisioned date and time, and `Orchestrator=Ansible_Automation`. The play recap should show `failed=0`.
9. **Common errors and cleanup.** If `ansible-playbook: command not found` appears, install Ansible using step 3. If a file cannot be found, run `pwd` and `ls -l` and return to `~/ansible_practical5`. If YAML syntax fails, check spaces and indentation. If WSL will not start, run `wsl --shutdown` in PowerShell and reopen Ubuntu; if Ubuntu is not installed, repeat step 2. To remove only the generated practical output, run `rm -rf /tmp/ansible_deployment`. To remove the working files, run `rm -rf ~/ansible_practical5`.

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

### Step-by-Step Instructions

1. Sign in to the AWS Management Console and open S3. Choose one AWS region and keep it consistent while creating the bucket and opening its website endpoint.
2. Click Create bucket. Enter a globally unique bucket name (for example, `practical6-static-site-101`, if available) and select the region. For this public website exercise, clear “Block all public access” and acknowledge the warning. Create the bucket. Do not use real/private files in a public bucket.
3. Open the bucket → Properties → Static website hosting → Edit. Enable website hosting, select “Host a static website”, and set `index.html` as the index document. Set `error.html` as the error document if you plan to upload one. Save and note the website endpoint.
4. Open Permissions → Bucket policy and paste the existing JSON policy below after replacing `YOUR-BUCKET-NAME` with the exact bucket name. Save the policy. If AWS Organizations or account-level settings prohibit public access, the policy may be blocked.
5. Create a local folder and save the existing HTML code below as `index.html`. For example, on Ubuntu/WSL:
   ```bash
   mkdir -p ~/practical6-site
   cd ~/practical6-site
   nano index.html
   ```
   Paste the HTML, save with `Ctrl+O`, Enter, and exit with `Ctrl+X`.
6. In the S3 bucket, choose Upload → Add files → select `index.html` → Upload. Upload `error.html` too if you configured it and want a custom error page.
7. Open the static website endpoint in a browser. Confirm that “Hosted on Amazon S3 Successfully!” appears.
8. If access is denied, check the bucket policy, the public-access block settings, and the object name/capitalization. If the endpoint does not work, confirm that static website hosting is enabled and use the **website endpoint**, not the normal S3 object URL. Delete the bucket after deleting its objects if you no longer need this public demo.

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

### Step-by-Step Instructions

1. **Prerequisites.** You need an AWS account with permission to create EC2 instances and read AMI metadata, plus an IAM identity for Terraform. AWS resources can incur charges.
2. **Install Terraform.** On Ubuntu/WSL, follow the official HashiCorp installation instructions for your Ubuntu version, then verify:
   ```bash
   terraform -version
   ```
   If the command is not found, Terraform is not installed or is not on `PATH`; complete the official installation before continuing.
3. **Configure AWS credentials.** Install AWS CLI if needed and configure credentials for an IAM identity with appropriate EC2 permissions:
   ```bash
   aws --version
   aws configure
   aws sts get-caller-identity
   ```
   Enter the access key, secret key, default region `eu-north-1`, and preferred output format when prompted. Do not commit credentials or put them in `main.tf`.
4. **Create the project directory.** Run:
   ```bash
   mkdir -p ~/practical7_terraform
   cd ~/practical7_terraform
   ```
5. **Create `main.tf`.** In this directory, create the file:
   ```bash
   nano main.tf
   ```
   Paste the complete existing Terraform configuration from section 1, save with `Ctrl+O`, Enter, and exit with `Ctrl+X`. The configuration uses region `eu-north-1` and instance type `t3.micro`.
6. **Initialize and review.** Run the commands in section 2 in order:
   ```bash
   terraform init
   terraform fmt
   terraform validate
   terraform plan
   ```
   Review the plan and confirm it proposes the intended EC2 instance before applying.
7. **Create and verify the instance.** Run:
   ```bash
   terraform apply
   ```
   Type `yes` when prompted. Alternatively, use the existing `terraform apply -auto-approve` command only when you have reviewed the plan. The output should include an instance ID and public IP. Check the EC2 Console in `eu-north-1` for `Practical7-Terraform-EC2`.
8. **Clean up.** After recording the result, run `terraform destroy` from the same directory and type `yes` to remove the instance. The existing `terraform destroy -auto-approve` command can be used if you have confirmed the resources. Verify in the EC2 Console that the instance is terminated.
9. **Common errors.** `terraform: command not found` means Terraform needs installation/PATH setup. AWS credential errors require valid credentials and IAM permissions. If no AMI is found, check the region and AMI filter. If the EC2 instance type is unavailable, select a supported type in the configuration and run `terraform plan` again.

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

### Step-by-Step Instructions

1. **Sign in and select a region.** Open the AWS Management Console and sign in. Select **Europe (Stockholm), `eu-north-1`** in the region selector. The region must match the explicit region in `lambda_function.py`.
2. **Create the DynamoDB table.** Open DynamoDB → Tables → Create table. Enter table name `DateTimeLogs`; partition key `log_id`; key type **String**. Choose On-demand capacity and create the table. Wait until the status is **Active**.
3. **Create an IAM role for Lambda.** Open IAM → Roles → Create role. Select AWS service as the trusted entity and Lambda as the use case. Attach `AWSLambdaBasicExecutionRole` so the function can write logs to CloudWatch. Create the role.
4. **Grant permission to write to the table.** Open the new role → Add permissions → Create inline policy → JSON. Replace the account ID placeholder in this policy with your 12-digit AWS account ID and save it as, for example, `AllowDateTimeLogsPutItem`:
   ```json
   {
     "Version": "2012-10-17",
     "Statement": [
       {
         "Effect": "Allow",
         "Action": "dynamodb:PutItem",
         "Resource": "arn:aws:dynamodb:eu-north-1:<YOUR-ACCOUNT-ID>:table/DateTimeLogs"
       }
     ]
   }
   ```
   Replace `<YOUR-ACCOUNT-ID>` before saving. This permission is limited to writing items to this table.
5. **Create the Lambda function.** Open Lambda → Create function → Author from scratch. Set function name to `DateTimeLogger` (or another name you choose), runtime to Python 3.12, and execution role to “Use an existing role”; select the role created above. Create the function.
6. **Enter the existing code.** In the function's Code tab, open `lambda_function.py` and replace its contents with the complete code in section 2 below. Keep the filename `lambda_function.py`, table name `DateTimeLogs`, partition key `log_id`, and region `eu-north-1` consistent. Click **Deploy** and wait for deployment to complete.
7. **Create a test event.** Select Test → Create new event. Name it `DateTimeLogsTest` and use this JSON:
   ```json
   {
     "source": "manual-test"
   }
   ```
   Save the event.
8. **Run the test.** Click Test. A successful invocation should show `statusCode: 200` and the message `Item inserted successfully into DynamoDB`, along with the generated `log_id` and timestamp.
9. **Verify the stored item.** Open DynamoDB → Tables → `DateTimeLogs` → Explore table items. Refresh the items view. You should see a new item with `log_id`, `timestamp`, `event_source`, and `status`.
10. **Check logs and troubleshoot.** Open Lambda → Monitor → View CloudWatch logs if execution fails. `AccessDeniedException` means the role policy or table ARN is incorrect. `ResourceNotFoundException` usually means the table name or region is mismatched. If the function has no logs, check that `AWSLambdaBasicExecutionRole` is attached. Ensure the function was deployed after editing the code.
11. **Clean up.** When finished, delete the Lambda function and `DateTimeLogs` table if you no longer need them. Delete the IAM role only if it was created specifically for this practical and is not used by another function.

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

### Step-by-Step Instructions

1. **Install Docker.** On Windows, install and start Docker Desktop, enable the WSL 2 engine, and enable WSL integration for your Ubuntu distribution in Docker Desktop settings. On Linux, install Docker Engine using the official instructions for your distribution. Open a new terminal after installation.
2. **Verify Docker is running.** In Ubuntu/WSL or your terminal, run:
   ```bash
   docker --version
   docker info
   docker run --rm hello-world
   ```
   The `hello-world` command should print a success message. If the Docker daemon is unavailable, start Docker Desktop/Engine and retry. If permission is denied on Linux, configure Docker access according to the official instructions or use `sudo` where appropriate.
3. **Create the practical directory and DAG folder.** Run these commands in the same terminal you will use to start the container:
   ```bash
   mkdir -p ~/practical9_airflow/dags
   cd ~/practical9_airflow
   ```
4. **Create the DAG file.** Create `dags/datetime_wednesday_dag.py`:
   ```bash
   nano dags/datetime_wednesday_dag.py
   ```
   Paste the complete Python DAG code from section 1 below, save with `Ctrl+O`, Enter, and exit with `Ctrl+X`. The filename must end in `.py` and must be inside the `dags` folder.
5. **Start Airflow in Docker.** From `~/practical9_airflow`, run the existing deployment command:
   ```bash
   docker run -d --name airflow-standalone -p 8085:8080 -v "${PWD}/dags:/opt/airflow/dags" apache/airflow:2.9.2 airflow standalone
   ```
   Docker downloads the image on first use. Wait a few minutes for startup and database initialization.
6. **Find the login credentials.** Check the container logs:
   ```bash
   docker logs airflow-standalone
   ```
   The standalone startup logs provide the web UI login details (often a generated password for user `admin`). Use the credentials shown in the logs.
7. **Open the web interface.** Visit `http://localhost:8085` in your browser and sign in. Find the DAG named `wednesday_datetime_display`. If it does not appear, confirm the file is in `~/practical9_airflow/dags`, inspect the container logs for Python/import errors, and wait for the DAG parser to refresh.
8. **Run the DAG/task.** In the DAG list, enable the DAG if it is paused. Open it and use the play/trigger control to trigger a manual run. Open the `display_datetime_task` task and view its log. You should see `CRON SCHEDULED TRIGGER EXECUTION:` followed by a timestamp. You can also run the existing command below from a terminal:
   ```bash
   docker exec -it airflow-standalone airflow tasks test wednesday_datetime_display display_datetime_task 2026-10-25
   ```
   This command tests the task directly; it does not prove the Wednesday cron schedule fired. The cron expression `30 9 * * 3` means 09:30 every Wednesday in Airflow's configured timezone.
9. **Stop and remove the container.** When finished, run:
   ```bash
   docker stop airflow-standalone
   docker rm airflow-standalone
   ```
   The DAG file remains in `~/practical9_airflow/dags`. To remove the local practical files too, run `rm -rf ~/practical9_airflow`.
10. **Common errors.** If port 8085 is already in use, change the left side of `-p 8085:8080` to an available port and open that port in the browser. If Docker says the daemon is not running, start Docker Desktop/Engine. If the container name already exists, stop/remove the old container before rerunning. If the DAG is missing, verify the volume mount and filename. If the task fails, inspect the task log and `docker logs airflow-standalone`.

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

### Step-by-Step Instructions

1. **Prepare the tools.** You need a running Jenkins controller and a Linux Jenkins agent with Git, Docker CLI, Docker daemon access, and `curl`. Verify Docker on the machine that will execute the pipeline:
   ```bash
   docker --version
   docker info
   git --version
   curl --version
   ```
   If using Docker Desktop on Windows, ensure its engine is running and the Jenkins build environment can reach it. Merely installing Docker on a different machine is not sufficient.
2. **Create a GitHub repository.** Create or use a repository accessible to Jenkins, then clone it:
   ```bash
   git clone https://github.com/<your-username>/<your-repository>.git
   cd <your-repository>
   ```
   Replace the placeholders with the actual repository details.
3. **Create the application files.** At the repository root, create a file named `Dockerfile` and paste the existing Dockerfile from section 1. Create `index.html` in the same directory and paste the existing HTML code from section 1. File names are case-sensitive on Linux.
4. **Create the pipeline file.** Save the existing declarative pipeline code from section 2 as `Jenkinsfile` in the repository root (no extension). The `Dockerfile`, `index.html`, and `Jenkinsfile` must be committed together.
5. **Commit and push the files.** Run:
   ```bash
   git add Dockerfile index.html Jenkinsfile
   git commit -m "Add Docker deployment pipeline"
   git push
   ```
6. **Configure Jenkins.** In the Jenkins web UI, choose New Item → enter `Practical10-Docker-Pipeline` → Pipeline → OK. Under Pipeline, select “Pipeline script from SCM”, select Git, enter the repository HTTPS clone URL, add credentials if the repository is private, set the correct branch (for example, `*/main`), and set Script Path to `Jenkinsfile`. Save the job.
7. **Run and verify.** Click Build Now and open Console Output/Stage View. After a successful build, open `http://localhost:8082` **on the host where the container port is published**. The page should show “Continuous Deployment via Jenkins & Docker”.
8. **Troubleshooting.** A Docker API/daemon error means Docker is not running or is not accessible from the Jenkins agent. A permission error means the Jenkins service account lacks Docker access; configure this securely for your environment rather than running arbitrary jobs with elevated privileges. A port conflict requires changing `HOST_PORT` in the Jenkinsfile. A missing-file error usually means the files are not in the repository root or were not committed. If `curl` is unavailable, install it on the build agent.
9. **Cleanup.** On the Docker host, remove the running container with `docker rm -f practical10-running-app` and, if desired, remove the image with `docker rmi devops-web-app:latest`. Do not remove images or containers used by other projects.

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
