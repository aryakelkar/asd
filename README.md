# asd
#  Ubuntu / WSL
# sudo apt update && sudo apt install -y git

# {Configure User Identity
# git config --global user.name "Your Name"
# git config --global user.email "your-email@example.com"

# Verify installation
# git --version
# mkdir git-practical-demo && cd git-practical-demo
# git init
# echo "DevOps Lab - Initial File Content" > app.txt
# git add app.txt
# git commit -m "Initial commit on main branch"
# echo "Appending update for difference check" >> app.txt
# git diff
# git branch feature-auth

# Switch branch
# git checkout feature-auth

# Add unique content
# echo "Feature authentication logic implemented" > auth.txt
# git add auth.txt
# git commit -m "Added authentication logic in feature branch"
# git checkout main
# git merge feature-auth
# git remote add origin https://github.com/<your-username>/git-practical-demo.git
# git branch -M main
# git push -u origin main

# Simulate syncing updates from remote
# git pull origin main
