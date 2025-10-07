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

## DVC Setup
Initialize DVC and track your data/models:
```bash
dvc init
dvc add ./data ./models/  # Add paths to files/folders you want to track
dvc remote add --default myremote gdrive://<your-folder-id>  # Replace with your Google Drive folder ID
dvc push
dvc status  # Check DVC status
```
- After `dvc add`, commit the generated `.dvc` files to Git: `git add .dvc` and `git commit/push`.

## Fixing DVC Push to Google Drive: "Access Blocked" Error

### The Problem
When trying to push data to Google Drive using the `dvc push` command, the operation fails with an error like "Access blocked" or "This app is blocked". This happens because Google blocks DVC's default authentication method, viewing it as an unverified or unsafe app, preventing access to your sensitive data.

### The Solution (Step by Step)
This solution involves creating a custom "app" in Google Cloud to make Google trust the request. It should take about 10-15 minutes.

#### 1. Set Up the Project in Google Cloud Console
- Go to [console.cloud.google.com](https://console.cloud.google.com/) and sign in with your Google account (the same one you'll use for Drive).
- Click **New Project** and name it something like "DVC-Google-Drive-Remote".
- From the left sidebar: **APIs & Services > Library**, search for "Google Drive API", and click **Enable**.

#### 2. Set Up the OAuth Consent Screen
- From **APIs & Services**, go to **OAuth consent screen**.
- Select **External** as the user type and click **Create**.
- Fill in **App name** (e.g., "DVC Drive Remote") and **User support email** (your email).
- On the **Test users** page: Click **+ Add Users** and add your email (this is a crucial step for it to work!).
- Click **Save and Continue** on each page.

#### 3. Create Credentials
- From **APIs & Services**, go to **Credentials**.
- Click **+ Create Credentials > OAuth client ID**.
- Choose **Desktop application** and name it "DVC Client".
- You'll get a **Client ID** and **Client Secret**—copy and save them securely.

#### 4. Apply Changes in DVC
- Open the terminal in your project folder (ensure your virtual env is activated if you have one).
- Find your remote name: `dvc remote list` (usually `myremote` or `origin`).
- Run these commands (replace `<remote-name>` with your remote name, and `YOUR-ID`/`YOUR-SECRET` with your values):
  ```
  dvc remote modify --local <remote-name> gdrive_client_id "YOUR-CLIENT-ID"
  dvc remote modify --local <remote-name> gdrive_client_secret "YOUR-CLIENT-SECRET"
  ```

#### 5. Run and Authenticate
- Run `dvc push` again.
- The browser will open automatically: Log in with the same email you added as a Test user, and approve the permissions (e.g., Drive API).
- Done—the push should succeed!

## Additional Notes
- **"Unable to acquire lock" Error**: This means a previous process got stuck. Close all terminals, or delete the file with: `rm .dvc/tmp/lock` (in PowerShell: `Remove-Item .dvc/tmp/lock`).
- **Activating Virtual Environment**: If needed, on Windows: `venv\Scripts\activate`; on macOS/Linux: `source venv/bin/activate`.
- **Security**: Don't share the ID or Secret in Git (add to `.gitignore`). If you change your Google password, you'll need to re-authenticate.
- **Sources**: Based on official DVC documentation [dvc.org](https://dvc.org/doc/user-guide/data-management/remote-storage/google-drive).

If you run into issues, try running `dvc push` in incognito mode, or share the error details! 😊# DVC Project Setup with Google Drive Remote

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

## DVC Setup
Initialize DVC and track your data/models:
```bash
dvc init
dvc add ./data ./models/  # Add paths to files/folders you want to track
dvc remote add --default myremote gdrive://<your-folder-id>  # Replace with your Google Drive folder ID
dvc push
dvc status  # Check DVC status
```
- After `dvc add`, commit the generated `.dvc` files to Git: `git add .dvc` and `git commit/push`.

## Fixing DVC Push to Google Drive: "Access Blocked" Error

### The Problem
When trying to push data to Google Drive using the `dvc push` command, the operation fails with an error like "Access blocked" or "This app is blocked". This happens because Google blocks DVC's default authentication method, viewing it as an unverified or unsafe app, preventing access to your sensitive data.

### The Solution (Step by Step)
This solution involves creating a custom "app" in Google Cloud to make Google trust the request. It should take about 10-15 minutes.

#### 1. Set Up the Project in Google Cloud Console
- Go to [console.cloud.google.com](https://console.cloud.google.com/) and sign in with your Google account (the same one you'll use for Drive).
- Click **New Project** and name it something like "DVC-Google-Drive-Remote".
- From the left sidebar: **APIs & Services > Library**, search for "Google Drive API", and click **Enable**.

#### 2. Set Up the OAuth Consent Screen
- From **APIs & Services**, go to **OAuth consent screen**.
- Select **External** as the user type and click **Create**.
- Fill in **App name** (e.g., "DVC Drive Remote") and **User support email** (your email).
- On the **Test users** page: Click **+ Add Users** and add your email (this is a crucial step for it to work!).
- Click **Save and Continue** on each page.

#### 3. Create Credentials
- From **APIs & Services**, go to **Credentials**.
- Click **+ Create Credentials > OAuth client ID**.
- Choose **Desktop application** and name it "DVC Client".
- You'll get a **Client ID** and **Client Secret**—copy and save them securely.

#### 4. Apply Changes in DVC
- Open the terminal in your project folder (ensure your virtual env is activated if you have one).
- Find your remote name: `dvc remote list` (usually `myremote` or `origin`).
- Run these commands (replace `<remote-name>` with your remote name, and `YOUR-ID`/`YOUR-SECRET` with your values):
  ```
  dvc remote modify --local <remote-name> gdrive_client_id "YOUR-CLIENT-ID"
  dvc remote modify --local <remote-name> gdrive_client_secret "YOUR-CLIENT-SECRET"
  ```

#### 5. Run and Authenticate
- Run `dvc push` again.
- The browser will open automatically: Log in with the same email you added as a Test user, and approve the permissions (e.g., Drive API).
- Done—the push should succeed!

## Additional Notes
- **"Unable to acquire lock" Error**: This means a previous process got stuck. Close all terminals, or delete the file with: `rm .dvc/tmp/lock` (in PowerShell: `Remove-Item .dvc/tmp/lock`).
- **Activating Virtual Environment**: If needed, on Windows: `venv\Scripts\activate`; on macOS/Linux: `source venv/bin/activate`.
- **Security**: Don't share the ID or Secret in Git (add to `.gitignore`). If you change your Google password, you'll need to re-authenticate.
- **Sources**: Based on official DVC documentation [dvc.org](https://dvc.org/doc/user-guide/data-management/remote-storage/google-drive).

If you run into issues, try running `dvc push` in incognito mode, or share the error details! 😊