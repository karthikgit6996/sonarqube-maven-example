pipeline {
    agent any
    
    environment {
        SONARQUBE_SERVER = 'sonarqube-server' // The name of your SonarQube server in Jenkins Global Tool Configuration
        PATH = "/opt/maven/bin:$PATH" // The name of your Maven installation in Jenkins Global Tool Configuration
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/karthikgit6996/sonarqube-maven-example.git'
            }
        }
        
        stage('Build') {
            steps {
                script {
                    def mvnHome = tool name: env.MAVEN_TOOL, type: 'maven'
                    withEnv(["PATH+MAVEN=${mvnHome}/bin"]) {
                        sh 'mvn clean install'
                    }
                }
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    withSonarQubeEnv(env.SONARQUBE_SERVER) {
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }
        
        stage('Quality Gate') {
            steps {
                script {
                    timeout(time: 5, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: true
                    }
                }
            }
        }
    }
}
