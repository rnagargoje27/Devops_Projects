This Docker Compose file sets up the following:
1. Jenkins with Docker-in-Docker capability through the dind service, allowing Jenkins to execute Docker commands and run buildx commands for multi-architecture builds.
2. Blue Ocean for a user-friendly Jenkins UI, configured to connect to the main Jenkins instance.
3. A shared data volume (jenkins_home) to persist Jenkins data across restarts.
4. Proper port mappings for Jenkins and Blue Ocean UI access.
5. DOCKER_TLS_CERTDIR set to empty to disable TLS, ensuring that Docker commands work without certificate issues.
To deploy this configuration, you need to have Docker and Docker Compose installed on your system. 
------------------------------------------------------------------------------------------------
Setup Docker & Docker compose:
Setting up Docker on a Linux system involves a few key steps: installing Docker, adding your user to the Docker group, testing the installation, and optionally installing Docker Compose. Here's a guide for installing Docker on a Linux system (using Ubuntu/Debian as a reference):
Step 1: Update the System
First, update your package list to ensure you're working with the latest repositories and packages:
sudo apt update && sudo apt upgrade -y
Step 2: Install Prerequisites
Ensure you have necessary tools and packages installed for adding new repositories:
sudo apt install -y apt-transport-https ca-certificates curl software-properties-common
Step 3: Add Docker’s GPG Key
Add Docker's official GPG key to ensure the authenticity of Docker packages:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
Step 4: Add Docker's Repository
Add Docker's repository to your system's package sources:
sudo add-apt-repository \
   "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) stable"
Step 5: Install Docker
Install Docker from the repository:
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io
Step 6: Start and Enable Docker Service
Start the Docker service and enable it to start on boot:
sudo systemctl start docker
sudo systemctl enable docker
Step 7: Add Your User to the Docker Group
To avoid using sudo with Docker commands, add your user to the Docker group. After doing this, you need to log out and back in for the group change to take effect:
sudo usermod -aG docker $USER
Step 8: Test Docker Installation
Run a simple Docker command to verify that Docker is installed and running correctly:
docker --version
docker run hello-world
If the "hello-world" container runs successfully, your Docker installation is complete.
Step 9: Install Docker Compose
Docker Compose is a tool to define and manage multi-container Docker applications. You can install it as follows:
sudo apt install docker-compose
docker-compose –version

That's it! You should now have Docker  and  Docker Compose  installed and ready to use on your Linux system
------------------------------------------------------------------------
Now use the following commands to start the services:
docker-compose up –d
This command starts all the defined services in detached mode. You can then access Jenkins at http://localhost:8080 and Blue Ocean at http://localhost:8081.





