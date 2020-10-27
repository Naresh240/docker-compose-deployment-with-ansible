pipeline {
    agent any
    parameters {
        choice(name: 'action', choices: 'docker-compose-up\ndocker-compose-down', description: 'docker compose up/down with jenkins')
    }
    tools { 
        maven 'maven3' 
    }
    stages {
        stage('SCM') {
            steps {
                sh 'cd /opt'
                checkout([$class: 'GitSCM', branches: [[name: '*/master']], 
                doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], 
                userRemoteConfigs: [[url: 'https://github.com/Naresh240/spring-boot-hello.git']]])
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('docker-deployment') {
    		when {
                expression { params.action == 'docker-compose-up' }
            }            
            steps {
                sh '''
                    cd /opt/docker-compose-deployment-with-ansible
                    sudo ansible-playbook docker-compose.yml --extra-vars deployment_state=started -e ansible_python_interpreter=/usr/bin/python3
                '''
            }
        }
        stage('docker-compose-down') {
    		when {
                expression { params.action == 'docker-compose-down' }
            }            
            steps {
                sh '''
                    cd /opt/docker-compose-deployment-with-ansible
                    sudo ansible-playbook docker-compose.yml --extra-vars deployment_state=absent -e ansible_python_interpreter=/usr/bin/python3
                '''
            }
        }
    }
}
