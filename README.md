# How to Deploy a Flask app using Gunicorn on an Ubuntu VM running on Microsoft Azure

If you want to display your Flask app during development on an Ubuntu virtual machine running on Azure, the default development server for Flask is not suitable. Gunicorn 'Green Unicorn' is a Python WSGI HTTP Server that can be used for the above purpose.

1. **Install Required Packages**:
   Ensure you have Python, pip, and virtualenv installed. Also, you'll need to install Gunicorn and Flask if you haven't already.

   ```sh
   sudo apt update
   sudo apt install python3 python3-pip python3-venv
   ```

2. **Set Up a Virtual Environment**:
   Create and activate a virtual environment for your Flask app.

   ```sh
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install Flask and Gunicorn**:
   Install Flask and Gunicorn within your virtual environment.

   ```sh
   pip3 install Flask gunicorn
   ```

4. **Prepare Your Flask App**:
   Ensure your `app.py` looks something like this:

   ```python
   from flask import Flask

   app = Flask(__name__)

   @app.route('/')
   def home():
       return "Hello World"

   if __name__ == "__main__":
       app.run(host='0.0.0.0', port=5000)
   ```

5. **Run Gunicorn**:
   Start your Flask app using Gunicorn.

   ```sh
   gunicorn --workers 3 --bind 0.0.0.0:8000 app:app
   ```

   This binds Gunicorn to all network interfaces on port 8000. Adjust the number of workers (`--workers`) based on the number of users you need to be able to access the application.

6. **Configure Firewall**:
   Ensure your Azure VM firewall and your Ubuntu firewall (if enabled) allow traffic on the necessary port (8000).

   For Azure VM:

   - Go to the Azure portal.
   - Navigate to your VM.
   - Click on "Networking".
   - Add an inbound port rule for port 8000.

   For Ubuntu firewall (ufw):

   ```sh
   sudo ufw allow 8000/tcp
   sudo ufw reload
   ```

7. **Access Your App**:
You should now be able to access your Flask app via the public IP address or domain name of your Azure VM on port 8000.

   ```sh
   http://<your_public_ip>
   ```

This setup will enable you to access your application from any machine via the public IP address of your Azure Virual Machine.