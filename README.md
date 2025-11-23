# durga-github-actions

# Github Repository:

# Create two branches:
git checkout -b main <br>
git push origin main <br>

git checkout -b staging <br>
git push origin staging <br>

# Create GitHub Actions Workflow Directory
.github/workflows/

# Create a new workflow file:
.github/workflows/ci-cd.yml

# Create the CI/CD Workflow File
ci-cd.yml

name: CI/CD Pipeline for Flask App

on:
  push:
    branches:
      - main
      - staging
  pull_request:
    branches:
      - main
      - staging
  release:
    types: [created]

jobs:

  install-and-test:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.10'

      - name: Install Dependencies
        run: pip install -r requirements.txt

      - name: Run Tests
        run: |
          pip install pytest
          pytest

  build:
    needs: install-and-test
    runs-on: ubuntu-latest
    if: success()

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Build Application
        run: |
          echo "Building application..."
          mkdir build
          cp -r app build/
          echo "Build complete!"

  deploy-staging:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/staging'

    steps:
      - name: Deploy to Staging
        run: |
          echo "Deploying to staging..."
          echo "Using secret key: ${{ secrets.STAGING_API_KEY }}"
          echo "Staging deployment done!"

  deploy-production:
    needs: build
    runs-on: ubuntu-latest
    if: startsWith(github.ref, 'refs/tags/')

    steps:
      - name: Deploy to Production
        run: |
          echo "Deploying to production..."
          echo "Using production key: ${{ secrets.PRODUCTION_API_KEY }}"
          echo "Production deployment done!"


# Add Environment Secrets

# Go to your repo: The workflow requires sensitive deployment credentials stored securely as GitHub Secrets.

Click on Settings → Secrets and variables → Actions → New repository secret <br>

Add the following secrets: <br>
STAGING_API_KEY	Used for staging deployment <br>
PRODUCTION_API_KEY	Used for production deployment <br>

These secrets are used in the workflow to authenticate deployment securely without exposing sensitive information in the code. <br>

# GitHub Actions CI/CD Workflow

This repository includes a GitHub Actions workflow to automate the Continuous Integration and Continuous Deployment (CI/CD) of the Flask application. <br>

The workflow performs the following steps automatically: <br>

- Installs the necessary Python dependencies. <br>
- Runs the test suite using pytest to ensure code quality. <br>
- Builds/prepares the application if tests pass. <br>
- Deploys the application to the **staging** environment on push to the `staging` branch. <br>
- Deploys the application to the **production** environment when a release is tagged. <br>

# Branch Usage:

- "**staging**" branch: This branch is used for deploying the application to the staging environment for testing before production release. <br>
- "**main**" branch or GitHub Releases: When a release tag is created (e.g., `v1.0.0`), the application is deployed to the production environment. <br>

<img width="1686" height="602" alt="image" src="https://github.com/user-attachments/assets/3c51e179-6a64-4eb9-b3f0-247e45629e74" />

# Install Dependencies:
Installed the required dependencies.

<img width="1391" height="843" alt="image" src="https://github.com/user-attachments/assets/fd5cdcf4-2cde-4ae2-b5a0-923db0f1b70f" />

# Run Tests:
All the tests are executed. <br>

<img width="1373" height="761" alt="image" src="https://github.com/user-attachments/assets/1d52ea02-9ca8-4222-810d-cd8d501aba2a" />

# Build Stage:
Build the application with Python code.

<img width="1415" height="823" alt="image" src="https://github.com/user-attachments/assets/19bd5724-2fcb-44d1-a529-e1ebe52cb851" />

# Deploy to Staging" triggered

<img width="762" height="817" alt="image" src="https://github.com/user-attachments/assets/d6fb6c0d-a271-4c66-99f4-4badbdf3eb5f" />

# How to test manually (locally running Flask app)
Start the Flask app (PowerShell) <br>

$env:MONGO_URI = "mongodb://localhost:27017/test_student_db" <br>
$env:SECRET_KEY = "test_secret_key" <br>
python app.py <br>

<img width="1408" height="522" alt="image" src="https://github.com/user-attachments/assets/cb5cc4b5-e361-43b3-bf0e-5178e55121eb" />

# Open a browser to http://127.0.0.1:5000 and use the "Add Student" form.

<img width="1675" height="558" alt="Screenshot 2025-11-23 152131" src="https://github.com/user-attachments/assets/43978630-9f57-480a-bdd2-25a85858a2ef" />

**Add Student**:

<img width="1546" height="708" alt="Screenshot 2025-11-23 152145" src="https://github.com/user-attachments/assets/eb8b2e01-1168-47da-8531-0f424e69c5ca" />

Once student details are added <br>

<img width="1687" height="428" alt="Screenshot 2025-11-23 152219" src="https://github.com/user-attachments/assets/7eeba325-2943-4a9d-9a07-105f107f8fcb" />


After adding the student details, update the course details:

<img width="1262" height="658" alt="image" src="https://github.com/user-attachments/assets/b66c925f-786d-458e-8f69-743f0ecaacf3" />

Updated the student details

<img width="1612" height="356" alt="image" src="https://github.com/user-attachments/assets/49453ca2-6f60-4ef6-925e-c883186cd0e9" />







