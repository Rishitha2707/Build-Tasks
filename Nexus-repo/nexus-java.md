1.🛠️ DevOps Project Setup: Maven Build + Nexus Repository

2.Nexus Repository Setup (ubuntu@nexus)

3.Create an instance for Nexus with All trafic security or create a bound rule with port 9000.

Connect the instance to the terminal.


 ✅ Step 1: System Prep & Java Installation
 ```bash
     sudo apt update
     java                # Check Java (not installed)
     sudo apt install openjdk-11-jre-headless
 ```

📥 Step 2: Download Nexus Repository Manager and extract it
   ```bash
  wget https://download.sonatype.com/nexus/3/nexus-unix-x86-64-3.79.0-09.tar.gz
  tar -xvf nexus-unix-x86-64-3.79.0-09.tar.gz
  ```

Move the exracted Nexus to /opt/ directory:
 ```bash
 sudo mv nexus-3* /opt/nexus
 sudo mv sonatype-work/ /opt/
```


👤 Step 3: Create a Dedicated User:
```bash
 sudo adduser nexus
```

Change the user permissions:
```bash
sudo chown -R nexus:nexus /opt/sonatype-work/
sudo chown -R nexus:nexus /opt/nexus
```


🔧 Step 4: Create Nexus as a Systemd Service
```bash
sudo vi /etc/systemd/system/nexus.service
```

Add the following content:
```bash
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
```


🚀 Step 5: Start and Enable Nexus
```bash
sudo systemctl enable nexus
sudo systemctl start nexus
sudo systemctl status nexus
```


✅ Useful Nexus URLs:
```bash
Nexus UI: http://<your-nexus-ip>:8081
```Default portnumber for nexus is 8081


You can see nexus server:

![Image](https://github.com/user-attachments/assets/31daf226-e22b-46e9-a263-4bc99bed48a5)


1.Give login credentials:
  It will give default credentials-
```bash
   Username:admin
   Password: is present in a path /opt/sonatype-work/nexus3/admin.password
```

2. Give new password and confirm password.



Part 2:
Build Server (ubuntu@Build-server)

 ✅ Step 1: System Preparation
    ```bash
     clear
     sudo apt update
    ``` 

☕ Step 2: Java Installation
    ```bash
    sudo apt install openjdk-11-jre-headless
    ```
Java runtime (JRE) is required for running Maven builds.


⚙️ Step 3: Install Maven
  ```bash
  sudo apt install maven -y
  ```
Maven is a build automation tool used for Java projects.


💻 Step 4: Optional Hostname Change:
```bash
 cd /etc/
 vi hostname       # (Fails, no sudo)
 sudo vi hostname  # Corrected
 sudo init 6       # Reboot system to apply hostname change
```
Optional step if you want to name the build server for clarity.




📦 Step 5: Clone Java Maven Project
```bash
 git clone https://github.com/Rishitha2707/DevOps-task.git
 cd DevOps-task/
 cd Java-code/
```

 🛠️ Step 6: Explore & Modify pom.xml
 Deploy From Java-Build to Nexus 
```bash
 vi pom.xml
```
You can copy the link from maven release URL as shown:
![Image](https://github.com/user-attachments/assets/d5577ca7-2264-4fd6-a1eb-7124410f62d0)

Update pom.xml by including the following:
```bash
</dependencies>
<distributionManagement>
  <repository>
    <id>maven-releases</id>
    <url>http://172.178.116.75:8081/repository/maven-releases/</url>
  </repository>
</distributionManagement>
```
You opened the Maven configuration file to check dependencies, plugins, and possible deploy settings.



📁 Step 7: Configure Maven for Nexus Deployment
```bash
sudo vi /etc/maven/settings.xml
```

In this file, you configure the following in <servers>:
 ```bash
 <servers>
  <server>
    <id>nexus</id>
    <username>admin</username>
    <password>your-nexus-password</password>
  </server>
</servers>
```

🧪 Step 8: Build and Package the App:
 ```bash
   mvn clean
   mvn package
```
mvn clean clears previous builds
mvn package compiles code and creates a .jar file

📤 Step 9: Deploy to Nexus
 ```bash
  mvn deploy
 ```
After successful build and correct credentials in settings.xml, Maven uploads artifacts to Nexus.

Refresh the nexus server
Go to browse
You can see the build java code here as:

![Image](https://github.com/user-attachments/assets/f0eb9336-4f73-4479-b4d1-93729900bb78)

![Image](https://github.com/user-attachments/assets/0aa2b681-b8ca-4988-adb9-fdd69e2b7e1b)
