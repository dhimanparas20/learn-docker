# Domain Pointing, SSL Setup, and Flask Hosting Guide

The main goal of this GitHub repository is to guide you through the process of pointing your domain "mstservices.online" (bought from Hostinger) to a virtual machine (VM) running a Flask server. Additionally, we'll set up SSL certificates from Let's Encrypt using Certbot to ensure secure connections.

- I hope you are smart enough to change the domain name, addresses and figure out basic things by yourself.

## Steps to Achieve the Goal

### 1. Domain Configuration
- Point your domain "mstservices.online" from your domain provider to the external IP address of the VM running on Google Cloud Platform (GCP).
- Alternatively, you can generate nameservers using GCP DNS. Obtain the nameservers from GCP DNS and update them at your domain provider.

### 2. Flask Project Setup
- Access your VM/server and create a Flask project running on a desired port (e.g., 5000).
- Configure Flask to listen on all interfaces by setting the host to '0.0.0.0'. This allows the server to accept connections from any IP address.

### 3. Firewall Configuration (GCP)
- If hosting on GCP, create a firewall rule to allow all incoming connections to the VM's external IP address.
- This step ensures that your VM responds to user requests by allowing incoming connections from the Internet.

### 4. Install Nginx, Python3, and Docker
```bash
sudo apt update
sudo apt install nginx python3 python3-pip 
```

### 5. Nginx Configuration
- Edit "/etc/nginx/sites-available" and create a configuration file (e.g., mstservices) to handle HTTP and HTTPS requests.
- Sample Nginx configuration (details in the repository's [nginx.conf](https://github.com/dhimanparas20/learn-docker.git) file).

### 6. Enable Nginx Configuration
```bash
sudo ln -s /etc/nginx/sites-available/mstservices /etc/nginx/sites-enabled/
```

### 7. Test Nginx Configuration
```bash
sudo nginx -t
```

### 8. Stop Nginx
- The next step required port 80 which is alredy aquired by nginx hence we need to stop that for a while.
```bash
sudo systemctl stop nginx
```

### 9. Obtain SSL Certificate
- Keep your certificate and key safe and private
- certificate will be generated at: /etc/letsencrypt/live/mstservices.online/fullchain.pem
- key will be generated at : /etc/letsencrypt/live/mstservices.online/privkey.pem
- You need to add them in you nginx config file, for refrence see  [nginx.conf](https://github.com/dhimanparas20/learn-docker.git)
```bash
sudo certbot certonly --standalone -d mstservices.online -d www.mstservices.online
```

### 10. Start Nginx
```bash
sudo systemctl start nginx
```

### 11. Build Docker Image
- Go to the project directory
```bash
docker build -t flask-app .
```

### 12. Run Docker Container
```bash
docker run -d -p 5000:5000 flask-app
```

### 13. Done
- Now, your domain "mstservices.online" should call the Flask server, and the HTTPS site should work fine with the SSL certificate.
- You just built a production ready server :-)
- Happy coding!
