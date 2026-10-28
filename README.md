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
