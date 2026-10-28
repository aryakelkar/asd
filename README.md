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


# DynamoDB
  import json
import boto3

#Initialize the DynamoDB resource
dynamodb = boto3.resource('dynamodb')
#Select your DynamoDB table
table = dynamodb.Table('kelkar')

def lambda_handler(event, context):
    # Extract data from the event object
    user_id = event.get('userID')
    name = event.get('name')
    email = event.get('email')
    phone = event.get('phone')

    # Put the item into the DynamoDB table
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
