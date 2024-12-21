Page URL: https://akor.web.mooo.com/
Public IP address: 3.253.58.211
                                          PROVISIONING THE SERVER                                          
Steps used in Setting Up a Linux Server using Ubuntu on Amazon Web Services (AWS):
     1. Logged into my AWS account.
     2. Navigated to the EC2 Dashboard to launch an EC2 instance (Ubuntu Server).
     3. Clicked "Launch Instance" to start creating the server.
     4. Named the instance "Second-Sem-Project".
     5. Selected Ubuntu Server as the operating system. 
     6. Chose the latest version (Ubuntu 22.04 LTS) that is eligible for the free tier.
     7. Selected the instance type (t2 micro free tier).
     8. Created a key pair, which automatically downloaded.
     9. Added a default storage of 8GB EBS volume.
     10. Created a security group and configured the following inbound and outbound rules:
          - SSH: allowed inbound traffic on port 22 for SSH access
          - HTTP: allowed inbound traffic on port 80 for web traffic
          - HTTPS: allowed inbound traffic on port 443 for SSL certificate configuration
          - All ICMP - IPv4: allowed site pinging on the terminal
          - Outbound rule: allowed all traffic
     11. Saved the security group and launched the instance.
     
   CONNECTING TO MY EC2 INSTANCE VIA SSH
Steps followed to Connect to the EC2 Instance via SSH:
      1. Navigated to the newly created instance and clicked on "Connect".
      2. Selected "SSH client" as the connection method.
      3. Copied the SSH command provided under "Example":ssh -i "second-sem-pro-keypair.pem" ubuntu@ec2-3-253-58-211.eu-west-1.compute.amazonaws.com
      4. Opened the Bash terminal.
      5. Created a new directory (second-semester-project) using mkdir and moved the downloaded key pair from the "Downloads" directory to the new directory.
      6. Changed the directory to the newly created one using cd second-semester-project, where the key pair was located.
      7. Ran the command chmod 400 "second-sem-pro-keypair.pem" to grant necessary permissions to the key pair.
      8. Pasted and executed the SSH command copied in step 3 to establish a secure connection to the EC2 instance.
                                                .
                                                INSTALLING NGINX WEB SERVER 
To install the Nginx web server, the following commands were executed:
    1. sudo apt update: to update the package list to ensure the latest versions were available.
    2. sudo apt upgrade -y: to upgrade the system to apply the latest security patches and updates.
    3. sudo apt install nginx: to install the Nginx web server.
    4. sudo systemctl status nginx: to verify the status of the Nginx service; it was runnig, If it wasn't, the command sudo systemctl start nginx would have been used to start it.
            Testing the Web Server:
Before proceeding with deployment, the web server was tested by:
1. Navigating to the EC2 instance details page,
2. Copyied the public IP address, and
3. Pasted the IP address into a web browser's URL bar.
The browser displayed the default Nginx webpage, confirming that the web server was up and running successfully.
                                            .
                                             DEPLOYING THE HTML PAGE
To deploy the HTML page to the server, the following steps and commands were executed:
    1. Created the landing page (index.html) and biography page (biography.html) using nano index.html and nano biography.html.
    2. Created a root directory for the website using sudo mkdir -p /var/www/my-website/html.
    3. Moved the HTML pages to the newly created site root directory using:
        - sudo mv index.html /var/www/my-website/html
        - sudo mv biography.html /var/www/my-website/html

    4. Verified file availability using ls /var/www/my-website/html.
    5. Set the correct permissions using:
        - sudo chown -R $USER:$USER /var/www/my-website
        - sudo chmod -R 755 /var/www/my-website
    6. Configured Nginx by creating a new server block configuration file for the website using: sudo nano /etc/nginx/sites-available/my-website.
    7. Added the following configuration:
server {
    listen 80;
    server_name 3.253.58.211;
    root /var/www/my-website/html;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
saved and exit.
      8. Created a symbolic link to enable the new configuration using: sudo ln -s /etc/nginx/sites-available/my-website /etc/nginx/sites-enabled/
      9. Tested the Nginx configuration for errors using: sudo nginx -t
      10. Reloaded the Nginx service to apply the changes using: sudo systemctl restart nginx
                                              .
                                              CONFIGURING HTTPS FOR MY WEB SERVER USING AN SSL CERTIFICATE
To configure HTTPS for my web server using a free SSL certificate from Let's Encrypt, the following steps were followed:
    1. Domain Creation with Afraid.DNS:
        -Signed up on afraid.dns,
        -Navigated to the "Subdomain" section and created a domain:
            -Selected Type as A.
            -Inputted the subdomain name as akor.web.
            -Chose mooo.com (a public domain) as the parent domain.
            -Inputted my server's public IP address as the destination.
            -saved
    2. The following ccommands were executed;
            -sudo apt update ..... to install the neccessary site packages
            -sudo apt install certbot python3-certbot-nginx ......to install certbot
            -sudo certbot --nginx .....to obtain SSL certificate- agreed to the terms, inputed my email and my domain name
