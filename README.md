
# Local Web Project

This repository contains a local web project setup using Vagrant for virtualization. It simplifies the setup process by utilizing Oracle VM Virtualbox, Vagrant, Git, Maven, and Corretto11jdk as prerequisites.

## Prerequisites

Before getting started, ensure you have the following prerequisites installed:

1. [Oracle VM Virtualbox](https://www.virtualbox.org/)
2. [Vagrant](https://www.vagrantup.com/)
3. [Git](https://git-scm.com/)
4. [Maven](https://maven.apache.org/)
5. [Corretto11jdk](https://aws.amazon.com/corretto/)

To streamline the installation process, you can use Chocolatey on Windows. Here's how you can install the prerequisites via the terminal:

```bash
choco install virtualbox vagrant git maven amazoncorretto11
```

## Setup Diagram
![setup](https://github.com/aclockworkobi9/Local_Web_Project/assets/146419037/f909beb1-8f56-4b1e-a4da-8281b52c19e0)

## Sequence of the Setup
![sequence](https://github.com/aclockworkobi9/Local_Web_Project/assets/146419037/3d62b991-45ff-4a7a-be9b-b70743114f30)

## Getting Started

To get started with this project, follow these steps:

1. **Clone this repository using Git:**

```bash
git clone https://github.com/aclockworkobi9/Local_Web_Project.git
```

2. **Navigate to the project directory:**

```bash
cd Local_Web_Project
```

3. **Locate the Vagrantfile and check if there are any VMs already running:**

```bash
vagrant global-status
```

4. **If no VMs are running, bring up the VM:**

```bash
vagrant up
```

If you encounter a hostmanager error, install the plugin using:

```bash
vagrant plugin install vagrant-hostmanager
```

## Login to the Database VM

To log in to the database VM, use the following command:

```bash
vagrant ssh db01
```

## Verify Hosts Entry

After logging in, verify the hosts entry to ensure it includes the necessary IP and hostnames:

```bash
cat /etc/hosts
```

If the entries are missing, update the `/etc/hosts` file accordingly.

## Update OS with Latest Patches

Before proceeding further, it's recommended to update the operating system with the latest patches. You can do this by running:

```bash
sudo yum update -y
```

## Set Repository

Install the EPEL repository to access additional packages:

```bash
sudo yum install epel-release -y
```

## Install MariaDB Package

Install the MariaDB server package:

```bash
sudo yum install git mariadb-server -y
```

## Starting & Enabling MariaDB Server

Start and enable the MariaDB server to ensure it runs on system boot:

```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

## Run MySQL Secure Installation Script

After MariaDB installation, it's recommended to secure the installation using the `mysql_secure_installation` script:

```bash
mysql_secure_installation
```

## Note

During the `mysql_secure_installation` process, you will be prompted to set the root password. For this guide, we'll use 'admin123' as the root password.

## Set Up Database Name and Users

After securing the installation, log in to MySQL as root user to create a database and grant privileges:

```bash
mysql -u root -padmin123
```

Inside MySQL, execute the following commands:

```sql
CREATE DATABASE accounts;
GRANT ALL PRIVILEGES ON accounts.* TO 'admin'@'%' IDENTIFIED BY 'admin123';
FLUSH PRIVILEGES;
EXIT;
```

## Download Source Code & Initialize Database

Clone the source code repository and initialize the database:

```bash
git clone -b main https://github.com/aclockworkobi9/Local_Web_Project.git
cd Local_Web_Project
mysql -u root -padmin123 accounts < src/main/resources/db_backup.sql
mysql -u root -padmin123 accounts
```

Inside MySQL, verify if tables are created:

```sql
SHOW TABLES;
EXIT;
```

## Restart MariaDB Server

After completing the database setup, restart the MariaDB server:

```bash
sudo systemctl restart mariadb
```

Now, your database is set up and ready to use!

## Log into the Memcache VM

To log in to the Memcache VM, use the following command:

```bash
vagrant ssh mc01
```

## Verify Hosts Entry

After logging in, verify the hosts entry to ensure it includes the necessary IP and hostnames:

```bash
cat /etc/hosts
```

If the entries are missing, update the `/etc/hosts` file accordingly.

## Update OS with Latest Patches

Before proceeding further, it's recommended to update the operating system with the latest patches:

```bash
sudo yum update -y
```

## Install, Start & Enable Memcache on Port 11211

Install the EPEL repository to access additional packages:

```bash
sudo dnf install epel-release -y
```

Install Memcached and start and enable it to run on system boot:

```bash
sudo dnf install memcached -y
sudo systemctl start memcached
sudo systemctl enable memcached
```

Check the status of Memcached to ensure it's running:

```bash
sudo systemctl status memcached
```

Modify the configuration to allow connections from any IP:

```bash
sudo sed -i 's/127.0.0.1/0.0.0.0/g' /etc/sysconfig/memcached
```

Restart Memcached for the changes to take effect:

```bash
sudo systemctl restart memcached
```

Now, your Memcached server should be up and running!

## Log into the RabbitMQ VM

To log in to the RabbitMQ VM, use the following command:

```bash
vagrant ssh rmq01
```

## Verify Hosts Entry



After logging in, verify the hosts entry to ensure it includes the necessary IP and hostnames:

```bash
cat /etc/hosts
```

If the entries are missing, update the `/etc/hosts` file accordingly.

## Update OS with Latest Patches

Before proceeding further, it's recommended to update the operating system with the latest patches:

```bash
sudo yum update -y
```

## Set EPEL Repository

Install the EPEL repository to access additional packages:

```bash
sudo yum install epel-release -y
```

## Install Dependencies

Install necessary dependencies including wget and RabbitMQ:

```bash
sudo yum install wget -y
cd /tmp/
dnf -y install centos-release-rabbitmq-38
dnf --enablerepo=centos-rabbitmq-38 -y install rabbitmq-server
systemctl enable --now rabbitmq-server
```

## Setup Access to User 'test' and Make It Admin

Configure access to the RabbitMQ server, create a user 'test', and grant administrative privileges:

```bash
sudo sh -c 'echo "[{rabbit, [{loopback_users, []}]}]." > /etc/rabbitmq/rabbitmq.config'
sudo rabbitmqctl add_user test test
sudo rabbitmqctl set_user_tags test administrator
sudo systemctl restart rabbitmq-server
```

Now, your RabbitMQ server is set up with a user 'test' having administrative privilege!

## Log into the Tomcat VM

To log in to the Tomcat VM, use the following command:

```bash
vagrant ssh app01
```

## Verify Hosts Entry

After logging in, verify the hosts entry to ensure it includes the necessary IP and hostnames:

```bash
cat /etc/hosts
```

If the entries are missing, update the `/etc/hosts` file accordingly.

## Update OS with Latest Patches

Before proceeding further, it's recommended to update the operating system with the latest patches:

```bash
sudo yum update -y
```

## Set Repository

Install the EPEL repository to access additional packages:

```bash
sudo yum install epel-release -y
```

## Install Dependencies

Install necessary dependencies including OpenJDK, Git, Maven, and wget:

```bash
sudo dnf -y install java-11-openjdk java-11-openjdk-devel
sudo dnf install git maven wget -y
```

## Change Directory to /tmp

Navigate to the temporary directory:

```bash
cd /tmp/
```

## Download Tomcat Package

Download and extract the Tomcat package:

```bash
wget https://archive.apache.org/dist/tomcat/tomcat-9/v9.0.75/bin/apache-tomcat-9.0.75.tar.gz
tar xzvf apache-tomcat-9.0.75.tar.gz
```

## Add Tomcat User

Add a dedicated user for Tomcat:

```bash
sudo useradd --home-dir /usr/local/tomcat --shell /sbin/nologin tomcat
```

## Copy Data to Tomcat Home Directory

Copy the extracted Tomcat files to the Tomcat home directory:

```bash
sudo cp -r /tmp/apache-tomcat-9.0.75/* /usr/local/tomcat/
```

## Make Tomcat User Owner of Tomcat Home Directory

Set the ownership of the Tomcat home directory to the Tomcat user:

```bash
sudo chown -R tomcat.tomcat /usr/local/tomcat
```

## Setup systemctl Command for Tomcat

Create a systemd service file for managing Tomcat:

```bash
sudo vi /etc/systemd/system/tomcat.service
```

Then paste the following contents into the file:

```
[Unit]
Description=Apache Tomcat Web Application Container
After=syslog.target network.target

[Service]
Type=forking

Environment=JAVA_HOME=/usr/lib/jvm/java-11-openjdk
Environment=CATALINA_PID=/usr/local/tomcat/temp/tomcat.pid
Environment=CATALINA_HOME=/usr/local/tomcat
Environment=CATALINA_BASE=/usr/local/tomcat

ExecStart=/usr/local/tomcat/bin/startup.sh
ExecStop=/usr/local/tomcat/bin/shutdown.sh

User=tomcat
Group=tomcat
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```

Save and exit the file.

## Reload systemd Files

After creating the systemd service file, reload the systemd daemon to apply changes:

```bash
sudo systemctl daemon-reload
```

## Start & Enable Service

Start the Tomcat service and enable it to start on boot:

```bash
sudo systemctl start tomcat
sudo systemctl enable tomcat
```

## Code Build & Deploy (app01)

### Download Source Code

Clone the source code repository:

```bash
git clone -b main https://github.com/aclockworkobi9/Local_Web_Project.git
```

### Update Configuration

Navigate to the project directory and update the configuration file `application.properties` with backend server details

:

```bash
cd Local_Web_project
vim src/main/resources/application.properties
```

### Build Code

Build the code inside the repository (`Local_Web_project`) using Maven:

```bash
mvn install
```

### Deploy Artifact

Stop the Tomcat service and deploy the artifact:

```bash
sudo systemctl stop tomcat
sudo rm -rf /usr/local/tomcat/webapps/ROOT*
sudo cp target/vprofile-v2.war /usr/local/tomcat/webapps/ROOT.war
sudo systemctl start tomcat
sudo chown -R tomcat:tomcat /usr/local/tomcat/webapps
sudo systemctl restart tomcat
```

Now, your updated code is built and deployed successfully on the app01 VM!

## Log into the Nginx VM

To log in to the Nginx VM, use the following command:

```bash
vagrant ssh web01
sudo -i
```

## Verify Hosts Entry

After logging in, verify the hosts entry to ensure it includes the necessary IP and hostnames:

```bash
cat /etc/hosts
```

If the entries are missing, update the `/etc/hosts` file accordingly.

## Update OS with Latest Patches

Before proceeding further, it's recommended to update the operating system with the latest patches:

```bash
apt update
apt upgrade
```

## Install Nginx

Install Nginx:

```bash
apt install nginx -y
```

## Create Nginx Configuration File

Create a new Nginx configuration file for the vproapp:

```bash
vi /etc/nginx/sites-available/vproapp
```

Then update it with the following content:

```
upstream vproapp {
    server app01:8080;
}

server {
    listen 80;

    location / {
        proxy_pass http://vproapp;
    }
}
```

## Remove Default Nginx Configuration

Remove the default Nginx configuration:

```bash
rm -rf /etc/nginx/sites-enabled/default
```

## Create a Link to Activate Website

Create a symbolic link to activate the website:

```bash
ln -s /etc/nginx/sites-available/vproapp /etc/nginx/sites-enabled/vproapp
```

## Restart Nginx

Restart Nginx to apply the changes:

```bash
systemctl restart nginx
```

Now, your Nginx server is configured to proxy requests to the vproapp running on app01 VM!

## Access the Web Application

To access the web application, follow these steps:

**1. Open a Web Browser:** 
   - Launch your preferred web browser on your computer.

**2. Enter the IP Address or Domain Name:**
   - In the address bar of your web browser, enter the IP address or domain name associated with your Nginx VM.
   - If you're using an IP address, it should look something like `http://xxx.xxx.xxx.xxx`.

**3. Accessing the Web Application:**
   - You should now be directed to the web application hosted on your Nginx VM. Interact with the web application as needed.

Now, you can access and use your web application through your web browser!

```
![validation1](https://github.com/aclockworkobi9/Local_Web_Project/assets/146419037/42e072cf-8a8c-4a6a-8ca8-fb689acd8ed3)
![validation](https://github.com/aclockworkobi9/Local_Web_Project/assets/146419037/ebc62b2d-f0c8-49e7-aaad-9c1a90a777bd)


This should provide a comprehensive guide for users to set up and access the local web project using GitHub-friendly markdown formatting. Let me know if you need further assistance!
