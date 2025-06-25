pipeline {
    agent any

    tools {
        jdk 'jdk11'
        maven 'maven3' // Make sure this is defined in Jenkins
    }

    environment {
        SCANNER_HOME = tool 'sonar-scanner' // Make sure this tool is defined
    }

    stages {
        stage('Git Checkout') {
            steps {
                git changelog: false, poll: false, url: 'https://github.com/shreyaalwal/secretsanta-generator.git'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') { // Match this name with what you set in Jenkins > SonarQube servers
                    sh '''
                        $SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.projectKey=shopping-cart \
                        -Dsonar.projectName=shopping-cart \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target/classes
                    '''
                }
            }
        }
    }
}
