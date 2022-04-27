pipeline {
    agent any
    
    tools {
        maven "mvn"
        jdk "jdk8"
    }
    
    stages {
        stage('Get code') {
            steps {
                git credentialsId: '5ecb6610-fa67-4c23-9ce4-85772582a569', url: 'https://github.com/Vinho0oNguyen/Maven.git'
            }
        }
        
        stage('Build file war'){
            steps {
                dir("/var/lib/jenkins/workspace/test-maven-pipeline") {
                    sh 'mvn -B -DskipTests clean package'
                }
            }
        }
        
        stage('Copy file to server'){
            steps{
                sh '''scp -i /home/jenkins/java-server.pem /var/lib/jenkins/workspace/test-maven-pipeline/target/sparkjava-hello-world-1.0.war www-user@192.168.1.26:/var/lib/tomcat/webapps
'''
            }
        }
        
        stage('Deploy'){
            steps{
                sh '''#!/bin/bash
                        ssh -i /home/jenkins/java-server.pem www-user@192.168.1.26 << EOF
                        sudo rm -Rf /var/lib/tomcat/webapps/sparkjava-hello-world-1.0
                        sudo service tomcat stop
                        sudo service tomcat start'''
            }
        }
    }
}
