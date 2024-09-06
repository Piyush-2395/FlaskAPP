GitHub Actions:

 First make a directory .github/workflow on GitHub.

Create environment secret on GitHub:
Open your Git Repo.
Go to settings.
Go below left side we have secret and environment.
Click on actions from dropdown.


Create main.yml inside the workflow

name: Python CI/CD Pipeline

on:
  push:
    branches:
      - main
  release:
    types:
      - created

jobs:
  install-dependencies:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'

      - name: Install Dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

  run-tests:
    runs-on: ubuntu-latest
    needs: install-dependencies
    steps:
      - uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'

      - name: Install Dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Run Tests
        run: |
          pytest

  build:
    runs-on: ubuntu-latest
    needs: run-tests
    steps:
      - uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.x'

      - name: Install Dependencies
        run: |
          python -m pip install --upgrade pip
          pip install -r requirements.txt

      - name: Build the Application
        run: |
          echo "Building the application..."

  deploy-to-ec2:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - name: Deploy to EC2
        uses: appleboy/ssh-action@v0.1.6
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ubuntu
          key: ${{ secrets.EC2_SSH_KEY }}
          port: 22
          script: |
            cd /home/ubuntu/FlaskAPP
            sudo apt update -y
            sudo apt install python3-pip python3-venv -y
            
            git pull origin main  # Pull the latest changes
            python3 -m venv myenv
            source myenv/bin/activate
            # Activate virtual environment (if needed)
            
            
            
            pip install -r requirements.txt  # Install dependencies
            python3 app.py
            
Screenshots of actions:



  <img width="468" alt="image" src="https://github.com/user-attachments/assets/94e921fe-9e6a-44d7-961f-e22f6377df83">
  <img width="468" alt="image" src="https://github.com/user-attachments/assets/b93238b3-254c-4551-9aa3-899b77d14646">

<img width="468" alt="image" src="https://github.com/user-attachments/assets/b71f2256-fed7-4d9e-ab0d-6e7450a3a6d8">
<img width="468" alt="image" src="https://github.com/user-attachments/assets/c47e2ba8-f1c6-4f6f-82b1-8cf7dde1ca60">


