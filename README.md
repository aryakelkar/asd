# asd
sudo apt update && sudo apt install -y git

 {Configure User Identity
 git config --global user.name "Your Name"
 git config --global user.email "your-email@example.com"

 Verify installation
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

 Add unique content
 echo "Feature authentication logic implemented" > auth.txt
 git add auth.txt
 git commit -m "Added authentication logic in feature branch"
 git checkout main
 git merge feature-auth
 git remote add origin https://github.com/<your-username>/git-practical-demo.git
 git branch -M main
 git push -u origin main

Simulate syncing updates from remote
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
