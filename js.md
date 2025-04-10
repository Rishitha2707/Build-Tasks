1. Install Git, Node.js, and Apache
First, you need to install Git, Node.js (for running the build process), and Apache (for serving the website). Use the following commands:

Install Git
Git is required to clone the repository and manage version control.
```bash
sudo apt update        # Update package index
sudo apt install git -y  # Install Git
git --version           # Check installed Git version
```


Install Node.js and NPM
Node.js and NPM are required to install dependencies and build the React app
```bash
sudo apt install npm -y  # Install NPM
npm --version            # Verify the installed version of NPM
node --version           # Verify the installed version of Node.js
```


Install Apache Web Server
Apache is used to serve the built version of the Portfolio website.
sudo apt install apache2 -y  # Install Apache2 web server
sudo systemctl status apache2  # Check Apache status to ensure it's running

Install Net-Tools
Net-tools will allow you to view network connections and port statuses
```bash
sudo apt install net-tools -y  # Install Net-Tools
sudo netstat -ntpl             # Check for active ports and listening services
```


2. Clone the Repository
Now that the required tools are installed, you can clone the Portfolio-Website repository from GitHub.
```bash
git clone https://github.com/jaiswaladi246/Portfolio-Website.git  # Clone the repository
```


Verify that the repository was cloned correctly by listing the contents of the current directory.
```bash
ls  # List files in the current directory to check if the 'Portfolio-Website' folder exists
```


Navigate to the repository folder:
```bash
cd Portfolio-Website/  # Change directory to the cloned repository
ls  # List contents of the Portfolio-Website folder
```


3. Install Dependencies and Build the Project
Before deploying the website, install the necessary dependencies and build the React app.

Install Node.js Dependencies
```bash
npm install  # Install all dependencies listed in package.json
```

Build the React App
```bash
npm run build  # Build the production-ready files
```


After running npm run build, the build directory will be generated in the project folder, containing the static files for deployment.

Check the contents of the build folder:
```bash
ls build/  # List files in the build directory to ensure build was successful
```

4. Set Up Apache to Serve the Website
Now that the project is built, it’s time to serve it using Apache.

Check Apache Status
```bash
sudo systemctl status apache2  # Check if Apache is running
```

Remove the Default Apache Index Page
Apache serves a default index.html page. You'll want to remove that and replace it with the Portfolio-Website build.
```bash
cd /var/www/html  # Change to Apache's web directory
ls  # List files in /var/www/html (you should see index.html here)
sudo rm index.html  # Remove the default index.html
```

Copy the Built Files to Apache's Web Directory
Now, copy the contents of the build folder to the Apache server's root directory (/var/www/html), where the web pages are served from.
```bash
cd  # Go back to the home directory
sudo cp -r Portfolio-Website/build/* /var/www/html  # Copy build files to /var/www/html
```


Verify that the files have been copied correctly:
```bash
ls /var/www/html  # List the files in /var/www/html to confirm the build files are there
```


5. Access the Website
After completing the steps above, your Portfolio website should now be live on your server, served by Apache.

Open a browser and go to the server's IP address or domain name.

```bash
Example:
http://3.91.151.120/
```
You should see the deployed Portfolio-Website


6. Additional Configuration (Optional)
If you want to make sure Apache automatically starts on boot, you can enable and start the service using:
```bash
sudo systemctl enable apache2  # Enable Apache to start on boot
sudo systemctl start apache2   # Start Apache service if it's not already running
```

Output:
![Image](https://github.com/user-attachments/assets/3bedb9d7-c1ea-406e-bdef-04192a9725bc)

![Image](https://github.com/user-attachments/assets/bbb0a779-7568-410d-a913-9d1568e8f4bf)

![Image](https://github.com/user-attachments/assets/fdd9e2c6-d555-4ab2-bfd6-bb925a33a6a9)







Explanation of Key Steps
1. Git Installation: Git is installed to manage the repository locally, allowing you to clone the Portfolio-Website repository and manage versions.

2. Node.js and NPM: These are used to install dependencies (from the package.json file) and build the app. The app is built into static files (HTML, CSS, JS) suitable for deployment.

3. Apache Setup: Apache is a widely-used web server that serves static files. In this case, after building the React app, Apache is used to serve the production-ready files (index.html and associated files).

4. Network Tools: Net-tools is installed to check network connections, ports, and services running on your server, ensuring Apache is running properly.

