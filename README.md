# DVC Project Setup with Google Drive Remote

This README guides you through initializing a Git repository, setting up DVC for data versioning, configuring a Google Drive remote, and fixing common authentication issues like the "Access Blocked" error.

## Git Setup
Initialize and push your code repository:
```bash
git init
git add .
git commit -m 'Initial commit'  # Replace with your message
git branch -M main  # Rename default branch to main if desired
git remote add origin <your-repo-url>  # SSH or HTTPS link, e.g., git@github.com:username/repo.git
git push -u origin main
git status  # Check status anytime
```
## DVC
```bash 
dvc init 
dvc add ./data ./models/   # what i want to track 
dvc remote add --default myremote gdrive://folder id 
dvc push 
dvc status 



```

