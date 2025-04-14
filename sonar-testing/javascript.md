🔐 Step 1: Connect to Your Ubuntu Server

📦 Step 2: Update and Install Required Software
Update System Package Index
sudo apt update

Verify Git Installation
git --version
If Git is not installed, run:
sudo apt install git

Install Java Runtime (Required for SonarQube)
sudo apt install openjdk-11-jre-headless

Install Node.js and NPM
sudo apt install npm

🧾 Step 3: Clone and Build JavaScript Project
Clone the Repository
git clone https://github.com/Rishitha2707/Build-Tasks.git


Navigate to JavaScript Project Directory
cd Build-Tasks/JS-code/

Install Project Dependencies
npm install
Build the Project
npm run build
This compiles or bundles the JavaScript code depending on the project setup (e.g., Webpack or other build tools).

🧰 Step 4: Install and Set Up SonarQube
Download SonarQube
wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-6.7.7.zip

List the Directory to Confirm the Download
ls

Install Unzip Utility
sudo apt install unzip

Extract SonarQube
unzip sonarqube-6.7.7.zip

Navigate to SonarQube Binary Folder
cd sonarqube-6.7.7/bin/linux-x86-64/

Start SonarQube Server
./sonar.sh start

Check SonarQube Status
./sonar.sh status

Use cd .. repeatedly to return to your home or project directory after setup.

🌐 Step 5: Access SonarQube in Browser
Open your web browser and go to:
http://3.85.118.219:9000/

Default SonarQube Credentials:
Username: admin

Password: admin

Upon first login, SonarQube will prompt you to:

Change the password

Create a new project

Select language (JavaScript), OS, and CI method (choose "Manually")


🔎 Step 6: Analyze Project with SonarScanner
Install SonarScanner
SonarScanner isn't included by default — install it with:
sudo apt install unzip curl
curl -O https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.8.0.2856-linux.zip
unzip sonar-scanner-cli-4.8.0.2856-linux.zip
sudo mv sonar-scanner-4.8.0.2856-linux /opt/sonar-scanner

Add it to your path:
echo 'export PATH=$PATH:/opt/sonar-scanner/bin' >> ~/.bashrc
source ~/.bashrc

Run SonarScanner with Token (from UI)
Paste the command provided by SonarQube after project creation:
sonar-scanner \
  -Dsonar.projectKey=js \
  -Dsonar.sources=. \
  -Dsonar.host.url=http://3.85.118.219:9000 \
  -Dsonar.login=b07b580b495569b149249672fe15c05ce714ecdd

✅ Step 7: View Code Quality Report
Return to the SonarQube browser tab.

Refresh the page.

You will now see the project listed.

Click on it to explore:
Bugs
Code Smells
Duplications
Maintainability metrics

