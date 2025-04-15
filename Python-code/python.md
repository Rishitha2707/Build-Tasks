In this guide, we'll go over the steps involved in creating a Python Flask application, setting it up with WSGI (Web Server Gateway Interface), running it with Gunicorn, and making it a backend service on a Linux virtual machine (VM).

1. Create a Virtual Machine (VM)
Create a Virtual Machine on a cloud provider (e.g., AWS, Google Cloud, Azure) or use a local virtual machine setup with Ubuntu Linux.

Connect to your VM using SSH:
```bash
ssh user@your-vm-ip
```

2. Update VM
Before installing any packages, make sure your VM is up-to-date by running:
```bash
sudo apt update  # Update package list
sudo apt upgrade -y  # Upgrade all the packages to the latest version
```


3. Install Dependencies
You'll need a few libraries and tools for Flask and other parts of the deployment process, like WSGI, Gunicorn, and Python development packages. Install them using:
```bash
sudo apt install -y libmariadb-dev gcc g++ build-essential python3-dev dh-python
```


4. Create the Flask Application Folder
Now, we’ll set up a directory for your Flask application and create a basic Flask app.
```bash
mkdir app1  # Create a new folder called 'app1'
cd app1  # Move into the 'app1' directory
vi main.py  # Open the vi editor to create a Python file
```


Inside the main.py file, write the following code to create a simple Flask application:
```bash
from flask import Flask

app = Flask(__name__)

@app.route('/app1')
def hello():
    return "Hello from App1"

if __name__ == "__main__":
    app.run(port=5001, debug=True)
```


Explanation:
This script initializes a basic Flask app with one route (/app1), which will return "Hello from App1" when accessed.
The app runs on port 5001 for development purposes (debug=True enables auto-reloading).


5. Install Python3 Virtual Environment Package
It is a best practice to use virtual environments to keep your project dependencies isolated. Install python3-venv if it's not already installed:
```bash
sudo apt install python3-venv
```

6. Create and Activate a Virtual Environment
Create a Python virtual environment and activate it:
```bash
python3 -m venv myenv  # Create a virtual environment called 'myenv'
source myenv/bin/activate  # Activate the virtual environment
```


You should see the (myenv) prefix in your terminal, indicating that the virtual environment is activated.

7. Install Flask within the Virtual Environment
With the virtual environment activated, install Flask:
```bash
pip3 install flask  # Install Flask inside the virtual environment
```


8. Run the Flask Application
Now, try running your Flask app to verify that everything is working. Run:
```bash
python3 main.py  # Run the Flask app
```


WARNING: Flask’s built-in development server is not suitable for production environments. The message will appear, saying:
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.

We'll use WSGI (Web Server Gateway Interface) and Gunicorn to handle this in production.

9. Set Up WSGI
Create a WSGI file (to be used by Gunicorn) to link the Flask application to the WSGI interface. Create a wsgi.py file:
```bash
vi wsgi.py  # Create a new file called wsgi.py
```


Inside wsgi.py, add the following code:
```bash
from main import app as application

if __name__ == "__main__":
    application.run(host='0.0.0.0', port=5001)
```


Explanation:

Here, the wsgi.py file imports the app from main.py and assigns it to application, which is the standard variable name expected by WSGI servers like Gunicorn.



10. Install Gunicorn
Now install Gunicorn, a Python WSGI HTTP server, to serve your Flask app:
```bash
pip3 install gunicorn  # Install Gunicorn
```


11. Run the Flask Application Using Gunicorn
Now that everything is set up, you can run your application using Gunicorn. Use the following command:
```bash
gunicorn --bind 0.0.0.0:5001 wsgi:app  # Start Gunicorn with binding to port 5001
```


Explanation:

--bind 0.0.0.0:5001 binds the app to all available IP addresses on port 5001.
wsgi:app tells Gunicorn to look for the app object in the wsgi.py file.
At this point, your app will be accessible at 
```bash
http://<VM_PUBLIC_IP>:5001/app1.
```


12. Create a Systemd Service to Run the Application as a Backend Service
To run the application as a service, we'll create a .service file for systemd. This will make sure the app runs automatically on startup and is managed as a system service.

Create the service file:
```bash
sudo nano /etc/systemd/system/app1.service  # Create the systemd service file
```


Add the following content:
```bash
[Unit]
Description=Gunicorn instance to serve app1
After=network.target

[Service]
User=ubuntu  # Replace with your actual username
Group=ubuntu # Replace with your actual username
WorkingDirectory=/home/ubuntu/app1  # Path to your app's directory
ExecStart=/home/ubuntu/app1/myenv/bin/gunicorn --workers 3 --bind 0.0.0.0:5001 wsgi:app  # Start the Gunicorn service

# Ensure proper shutdown
TimeoutStopSec=90
Restart=always
KillMode=mixed

[Install]
WantedBy=multi-user.target  # This makes sure the service is started at boot
```


Explanation:
ExecStart specifies the command that starts the Gunicorn server.
--workers 3 starts Gunicorn with 3 worker processes for better performance.
Restart=always ensures the service will restart if it crashes.
WantedBy=multi-user.target ensures the service is enabled during system boot.



13. Enable and Start the Service
Now that the service file is created, you can enable and start your Flask app as a background service using the following commands:
```bash
sudo systemctl daemon-reload  # Reload systemd to recognize the new service file
sudo systemctl enable app1  # Enable the service to start on boot
sudo systemctl start app1  # Start the service immediately
```


14. Check Service Status
To verify that your service is running correctly, check its status with:
```bash
sudo systemctl status app1  # Check the status of the app1 service
```


You should see something like:
```bash
● app1.service - Gunicorn instance to serve app1
   Loaded: loaded (/etc/systemd/system/app1.service; enabled; vendor preset: enabled)
   Active: active (running) since ...
   ...
```


15. Access the Application
Now your Flask app is running as a backend service on the VM, managed by systemd and Gunicorn.
open your web browser.
```bash
Go to http://<VM_PUBLIC_IP>:5001/app1.
```

You should see:
```bash
Hello from App1
```
Conclusion
This guide provides a step-by-step walkthrough of deploying a Flask application using WSGI, Gunicorn, and systemd as a backend service. By following these steps, you ensure that your application runs efficiently in a production environment, with Gunicorn managing the WSGI server and systemd handling the service lifecycle.
