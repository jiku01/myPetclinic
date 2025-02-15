pipeline {
    agent any 
    
    tools {
        jdk 'jdk17'
        maven 'maven3'
    }
    
    
    stages {
        stage("Git Checkout") {
            steps {
                git branch: 'main', changelog: false, poll: false, url: 'https://github.com/jiku01/myPetclinic.git'
            }
        }

        stage("Compile") {
            steps {
                sh "mvn clean compile"
            }
        }
        
        stage("Test Cases") {
            steps {
                sh "mvn test"
            }
        }

        stage("Build") {
            steps {
                sh "mvn clean install"
            }
        }
        
        stage("Docker Build & Push") {
            steps {
                script {
                   withDockerRegistry(credentialsId: '8d49b875-1775-4310-8970-22137617fff1', toolName: 'docker') {
                        sh "docker build -t image1 ."
                        sh "docker tag image1 jiku01/pet-clinic123:latest"
                        sh "docker push jiku01/pet-clinic123:latest"
                    }
                }
            }
        }

        stage("TRIVY") {
            steps {
                sh "trivy image --severity CRITICAL jiku01/pet-clinic123:latest"
            }
        }

        stage("Deploy To Tomcat") {
            steps {
                sh "cp /var/lib/jenkins/workspace/pipelinejob/target /opt/apache-tomcat-9.0.65/webapps/"
            }
        }
    }
}

