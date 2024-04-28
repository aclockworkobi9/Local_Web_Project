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
## Setup diagram
![setup](https://github.com/aclockworkobi9/Local_Web_Project/assets/146419037/f909beb1-8f56-4b1e-a4da-8281b52c19e0)
## Sequence of the setup
![sequence](https://github.com/aclockworkobi9/Local_Web_Project/assets/146419037/3d62b991-45ff-4a7a-be9b-b70743114f30)

## Getting Started

To get started with this project, follow these steps:

1. Clone this repository using Git:

```bash
git clone https://github.com/aclockworkobi9/Local_Web_Project.git
```

2. Navigate to the project directory:

```bash
cd Local_Web_Project
```

3. Locate the Vagrantfile and check if there are any VMs already running:

```bash
vagrant global-status
```

4. If no VMs are running, bring up the VM:

```bash
vagrant up
```

![globalstatus_and_error](https://github.com/aclockworkobi9/Local_Web_Project/assets/146419037/583c2d99-ef4d-4983-92e9-2e175dd7fc4e)
if you get hostmanager error then install the plugin with
```bash
vagrant plugin install vagrant-hostmanager
```


## Login to the Database VM

To log in to the database VM, use the following command:

```bash
vagrant ssh db01
```

### Verify Hosts Entry

After logging in, verify the hosts entry to ensure it includes the necessary IP and hostnames:

```bash
cat /etc/hosts
```

If the entries are missing, update the `/etc/hosts` file accordingly.

### Update OS with Latest Patches

Before proceeding further, it's recommended to update the operating system with the latest patches. You can do this by running:

```bash
sudo yum update -y
```

### Set Repository

Install the EPEL repository to access additional packages:

```bash
sudo yum install epel-release -y
```

### Install MariaDB Package

Install the MariaDB server package:

```bash
sudo yum install git mariadb-server -y
```

### Starting & Enabling MariaDB Server

Start and enable the MariaDB server to ensure it runs on system boot:

```bash
sudo systemctl start mariadb
sudo systemctl enable mariadb
```

Now, your MariaDB server should be up and running!
