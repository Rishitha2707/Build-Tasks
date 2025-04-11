🧾 Prerequisites
Before starting, make sure you have:
. Java
. Maven

Make sure to install the versions according to the requirement.

🔧 Step-by-Step Setup
1. Install Java
```bash
sudo apt update
sudo apt install openjdk-11-jre-headless -y
java --version
```
✅ You installed OpenJDK 11, which is required to run the project and Tomcat.




2. Install Maven
```bash
sudo apt install maven -y
```
✅ Maven is needed to build the project and generate the .war file.



3. Clone the GitHub Repository
```bash
git clone https://github.com/koddas/war-web-project.git
cd war-web-project
```
📁 This downloads the source code of the project to your local machine.




4. Build the Project Using Maven
```bash
mvn package
```
🛠️ This creates a .war file inside the target/ directory.

You can verify it:
```bash
ls target/
```
Look for something like war-web-project.war.



5.Download and Extract Apache Tomcat
```bash
wget https://dlcdn.apache.org/tomcat/tomcat-9/v9.0.104/bin/apache-tomcat-9.0.104.tar.gz
tar -xvf apache-tomcat-9.0.104.tar.gz
cd apache-tomcat-9.0.104
```
🧰 Apache Tomcat is the servlet container where we will deploy the WAR file.



6. Configure Tomcat Users for Web Manager Access
Edit the tomcat-users.xml file:
```bash
cd conf/
sudo vi tomcat-users.xml
```

Add the following inside <tomcat-users> tag:
```bash
<role rolename="manager-gui"/>
<role rolename="admin-gui"/>
<user username="admin" password="admin" roles="manager-gui,admin-gui"/>
```
🧑‍💻 This enables you to log into the web interface of the Tomcat Manager.

Also, make sure to allow remote access in:
```bash
sudo vi ../webapps/manager/META-INF/context.xml
sudo vi ../webapps/host-manager/META-INF/context.xml
```
Comment out or remove the IP-based restrictions:
```bash
<!-- <Valve className="org.apache.catalina.valves.RemoteAddrValve"
       allow="127\.\d+\.\d+\.\d+|::1|0:0:0:0:0:0:0:1" /> -->
```




7. Deploy WAR File to Tomcat
Copy the WAR file into the webapps/ folder:
```bash
sudo cp ~/war-web-project/target/*.war ~/war-web-project/apache-tomcat-9.0.104/webapps/
```
🚀 Tomcat will auto-deploy WAR files placed in webapps/.




8. Start the Tomcat Server
```bash
cd ~/war-web-project/apache-tomcat-9.0.104/bin
./startup.sh
```
🌐 This launches the Tomcat server. By default, it's accessible at http://<your-ip>:8080.

You can verify it:
```bash
curl http://<your-ip>:8080/manager
```

Or open in your browser:
```bash
http://<your-ip>:8080/manager/html
```
Use the credentials you set earlier (admin / admin).

![Image](https://github.com/user-attachments/assets/d164da65-6984-4560-a740-580033780113)
