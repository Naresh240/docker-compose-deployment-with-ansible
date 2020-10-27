# docker-compose-deployment-with-ansible

Pre-requisites:
---------
  - Install Java
  - Install Maven
  - Install Docker
  - Install Docker-compose
  - Install jenkins
# Ansible setup: (Master Server)
    amazon-linux-extras install ansible2 -y
    ssh-keygen
# Copy "/root/.ssh/id_rsa.pub" to Slave Machine at /root/.ssh/authorized_keys
# Keep slave server with in inventory file (Master Server)
    vi /etc/ansible/hosts
# Check wether we can ping slave with master or not
    ansible -m ping <Slave-Server-IP>
# Install Java, Maven, Docker and Docker-Compose With in Slave Machine
# Install java:
    yum install java-1.8.0-openjdk-devel -y
# Install Maven
    yum install maven -y
# Install jenkins:
    sudo wget -O /etc/yum.repos.d/jenkins.repo http://pkg.jenkins.io/redhat-stable/jenkins.repo
    sudo rpm --import http://pkg.jenkins.io/redhat-stable/jenkins.io.key
    sudo yum install jenkins -y
    service jenkins start
# Install Docker:
    yum install docker -y
    service docker start
    usermod -aG docker jenkins
# Restart Jenkins:
    service jenkins restart
# Provide sudo permission for jenkins user
    visudo
    --------
    jenkins ALL=(ALL)       NOPASSWD: ALL
    --------
# Install docker-compose
    yum install python3-pip -y
    pip3 install docker-compose 
# Create docker-compose.yml file inside /opt Directory
# Clone Springboot-mongodb application:
    git clone https://github.com/Naresh240/docker.git
    cd docker/springboot-mongodb-dockercompose
# Build artifact
    mvn clean install -DskipTests=true
# Build Docker Image
    docker build -t naresh240/springboot-mongodb:latest .
# Docker login
    docker login -u naresh240 -p
# Push Docker image to Dockerhub
    docker push naresh240/springboot-mongodb:latest
# Deploy docker-compose using ansible at Jenkins
Step1:
  Integreate Git and Maven with Jenkins
Step2:
  Create Docker Credentials
Step3:
  Create Jenkins Pipeline Job and copy jenkins pipeline inside pipeline section
 Step4:
  Build Jenkins job
# Use postman app and Add Employee data
Check API: /addEmployee

    http://<ip-address>:8080/addEmployee
  
In postman app keep Post method and give Json data by selecting Body --> raw (Json format)

    {"id": "1001", "name": "Naresh", "departement": "Engineer"}
    
Check API:- /findAllEmployees

Goto Web UI and check below link

    http://<ip-address>:8080/findAllEmployees
# Use postman app and Add Employee data
Check API: /addEmployee

    http://<ip-address>:8080/addEmployee
  
In postman app keep Post method and give Json data by selecting Body --> raw (Json format)

    {"id": "1001", "name": "Naresh", "departement": "Engineer"}
    
Check API:- /findAllEmployees

Goto Web UI and check below link

    http://<ip-address>:8080/findAllEmployees
