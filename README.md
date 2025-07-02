# AWS EC2 Automated Web Server Deployment

## Project Overview

This repository contains the configuration and script used to automatically deploy a basic web server (Nginx) on an AWS EC2 instance using *User Data*. This project demonstrates how to provision and configure cloud compute resources efficiently without manual intervention after the initial launch.

## Key Features & Technologies Used

* *AWS EC2:* Virtual servers in the cloud.
* *AWS User Data:* Scripting feature for automated instance configuration.
* *Ubuntu Server:* The chosen operating system for the EC2 instance.
* *Nginx:* A high-performance web server.
* *AWS Security Groups:* Network firewall for instance access control.
* *Bash Scripting:* For automating the installation and configuration tasks.

## Architecture

The architecture is straightforward:

1.  An EC2 instance is launched in a public subnet.
2.  During launch, User Data is passed to the instance.
3.  The User Data script installs Nginx and deploys a simple index.html page.
4.  A Security Group allows SSH (for management) and HTTP (for web access) traffic.


+----------------+       +-------------------+       +-------------------+
|     Internet   | ----> | AWS Security Group| ----> |   EC2 Instance    |
+----------------+       | (HTTP/SSH Allowed)|       | (Ubuntu, t3.micro)|
+-------------------+       | (Nginx Installed) |
|                   | (index.html Served)|
|                   +-------------------+
|                            ^
|                            |
+----------------------------+
User Data Script

## Deployment Instructions

Follow these steps to deploy your own automated web server:

### Prerequisites

* An active AWS Account.
* An AWS Key Pair (downloaded .pem file).
* Basic understanding of AWS EC2 and networking concepts.

### Step-by-Step Guide

1.  *Launch an EC2 Instance:*
    * Navigate to the EC2 Dashboard in your AWS Console and click "Launch instances."
    * Give your instance a name (e.g., MyAutomatedWebserver).
    * Select *Ubuntu Server 22.04 LTS (HVM) - Free tier eligible* as your AMI.
    * Choose t3.micro as the instance type (Free Tier eligible).

    ![AMI and Instance Type Selection](path/to/your/ami_instance_type_screenshot.png)
    (Note:* Replace path/to/your/... with the actual path or URL of your image once uploaded to GitHub or an image host.)*

2.  *Configure Key Pair:*
    * Select an existing key pair or create a new one (remember to download the .pem file if creating new).

    ![Key Pair Selection](path/to/your/key_pair_screenshot.png)

3.  *Set Up Security Group:*
    * Create a new Security Group (e.g., web-server-sg) or select an existing one with the necessary inbound rules.
    * Add an *SSH (Port 22)* rule (Source: My IP for security).
    * Add an *HTTP (Port 80)* rule (Source: Anywhere (0.0.0.0/0) for web access).

    ![Security Group Rules](path/to/your/security_group_screenshot.png)

4.  *Add User Data Script:*
    * Scroll down to the "Advanced details" section during instance launch.
    * Paste the following Bash script into the "User data" text box. This script will install and configure Nginx upon instance boot.

    bash
    #!/bin/bash
    sudo apt update -y
    sudo apt install nginx -y
    sudo systemctl start nginx
    sudo systemctl enable nginx
    echo "<h1>Hello from Ejike's Automated EC2 Web Server!</h1>" | sudo tee /var/www/html/index.html
    

    ![User Data Script](path/to/your/user_data_screenshot.png)

5.  *Launch the Instance:*
    * Review all your settings and click "Launch instance."

## Verification

1.  *Monitor Instance State:*
    * Navigate to the EC2 Instances page. Your MyAutomatedWebserver instance should transition to the running state within a few minutes.

    ![Running Instance on Dashboard](path/to/your/running_instance_dashboard_screenshot.png)

2.  *Access Web Server:*
    * Copy the *Public IPv4 address* of your running instance.
    * Paste this IP address into your web browser. You should see the "Hello" message.

    ![Live Website in Browser](path/to/your/live_website_screenshot.png)

## Clean Up (Important!)

To avoid incurring any charges, remember to *terminate your EC2 instance* after you are done experimenting or have confirmed the setup:

1.  Select your MyAutomatedWebserver instance on the EC2 Instances page.
2.  Go to "Instance state" -> "Terminate instance".
3.  Confirm the termination.

## Related Blog Post

For a more detailed walkthrough, deeper explanations, and insights into this project, check out my full blog post:

[*Automating Your First Server: Hosting a Web Page on AWS EC2 with User Data - GreenTechGuru Online*](https://greentechguruonline.wordpress.com/your-ec2-blog-post-url-here/)

---
