🛠️ DevOps Project Setup: Maven Build + Nexus Repository

 Nexus Repository Setup (ubuntu@nexus)

Create an instance for Nexus with All trafic security.

Connect the instance to the terminal.


 ✅ Step 1: System Prep & Java Installation
   sudo apt update
  java                # Check Java (not installed)
  sudo apt install openjdk-11-jre-headless


📥 Step 2: Download Nexus Repository Manager and extract it
9  wget https://download.sonatype.com/nexus/3/nexus-unix-x86-64-3.79.0-09.tar.gz
11 tar -xvf nexus-unix-x86-64-3.79.0-09.tar.gz


Move the exracted Nexus to /opt/ directory:
 sudo mv nexus-3* /opt/nexus
 sudo mv sonatype-work/ /opt/


👤 Step 3: Create a Dedicated User:
 sudo adduser nexus

Change the user permissions:
sudo chown -R nexus:nexus /opt/sonatype-work/
sudo chown -R nexus:nexus /opt/nexus


🔧 Step 4: Create Nexus as a Systemd Service
sudo vi /etc/systemd/system/nexus.service

Add the following content:
[Unit]
Description=nexus service
After=network.target

[Service]
Type=forking
LimitNOFILE=65536
User=nexus
Group=nexus
ExecStart=/opt/nexus/bin/nexus start
ExecStop=/opt/nexus/bin/nexus stop
Restart=on-abort

[Install]
WantedBy=multi-user.target


🚀 Step 5: Start and Enable Nexus
sudo systemctl enable nexus
sudo systemctl start nexus
sudo systemctl status nexus


✅ Useful Nexus URLs:
Nexus UI: http://<your-nexus-ip>:8081
Default portnumber for nexus is 8081

You can see nexus server:
1.Give login credentials:
  It will give default credentials-
   Username:admin
   Password: is present in a path /opt/sonatype-work/nexus3/admin.password

2. Give new password and confirm password.



 Build Server (ubuntu@Build-server)

 ✅ Step 1: System Preparation
     clear
     sudo apt update

☕ Step 2: Java Installation
    sudo apt install openjdk-11-jre-headless
Java runtime (JRE) is required for running Maven builds.


⚙️ Step 3: Install Maven
  sudo apt install maven -y
Maven is a build automation tool used for Java projects.


💻 Step 4: Optional Hostname Change:
 cd /etc/
 vi hostname       # (Fails, no sudo)
 sudo vi hostname  # Corrected
 sudo init 6       # Reboot system to apply hostname change

Optional step if you want to name the build server for clarity.




📦 Step 5: Clone Java Maven Project
git clone https://github.com/Rishitha2707/DevOps-task.git
 cd DevOps-task/
 cd Java-code/


 🛠️ Step 6: Explore & Modify pom.xml
 Deploy From Java-Build to Nexus 
vi pom.xml
Update pom.xml by including the following:
</dependencies>
<distributionManagement>
  <repository>
    <id>maven-releases</id>
    <url>http://172.178.116.75:8081/repository/maven-releases/</url>
  </repository>
</distributionManagement>

You opened the Maven configuration file to check dependencies, plugins, and possible deploy settings.

📁 Step 7: Configure Maven for Nexus Deployment
sudo vi /etc/maven/settings.xml

In this file, you configure the following in <servers>:
  <servers>
  <server>
    <id>nexus</id>
    <username>admin</username>
    <password>your-nexus-password</password>
  </server>
</servers>

🧪 Step 8: Build and Package the App:
   mvn clean
   mvn package

mvn clean clears previous builds
mvn package compiles code and creates a .jar file

📤 Step 9: Deploy to Nexus
   mvn deploy
After successful build and correct credentials in settings.xml, Maven uploads artifacts to Nexus.

Refresh the nexus server
Go to browse
You can see the build java code here as: