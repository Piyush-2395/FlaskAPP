Installing Jenkins on AWS VM:
sudo apt-get update.
sudo apt install openjdk-11-jre.
Java --version.
sudo mkdir -p /usr/share/keyrings/
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \ https://pkg.jenkins.io/debian binary/ | sudo tee \  /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update -y
sudo apt install jenkins.


Configure sshd service on jenkins server






Now check the jenkins password
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
Now change the jenkins password.





Required Plugins to set up the pipeline:

Manage jenkins → Plugins → publish over ssh, ssh agent, pipeline stage view


Add deployment Server into jenkins:




Creating Credentials on jenkins:



Configure Jenkins with Python and any necessary libraries   :
sudo apt update
sudo apt install python3 python3-pip
sudo apt install python3.12-venv → installing environment package
python3 -m venv myenv → This will create separate environment for FLASK app to run on EC2 VM.

source myenv/bin/activate → to switch into environment
pip install flask
Pip3 install pytest








Below we have provided password to the jenkins user:
cat /etc/passwd
sudo passwd jenkins



Provide root access to the jenkins user:
First login as a root sudo su - 
Second run command cd /etc/
Then nano sudoers and do the below changes


Login as a jenkins user and generate a private/public key:
Su jenkins
Password
Cd /var/lib/jenkins
Cd .ssh
Ssh-keygen
Key will get generated
Below Snapshot:



Open another VM on EC2 and do below things:
Ls -la
Cd .ssh
Sudo nano authorized_keys
Paste the public key of jenkins server
Below snap for reference 


To test the Password less authentication we logged in to ANother VM through jenkins VM



Create credentials on Jenkins Dashboard GUI → Manage Jenkins → credentials




Select job → configure → in Pipeline definition→ select git as SCM

