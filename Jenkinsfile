pipeline {
    agent any
    tools {
        maven 'M2_HOME'
    }
    environment {
    registry = '880385147960.dkr.ecr.us-east-1.amazonaws.com/geolocation_24'
    registryCredential = 'aws_jenkins'
    dockerimage = ''
    }
    stages{
        stage("Maven Build"){
            steps{
                sh 'mvn clean'
                sh 'mvn package'
            }
        }
        stage("Build & SonarQube Analysis"){
            steps{
                withSonarQubeEnv('sonarQube') {
                sh 'mvn verify sonar:sonar'
            }

        }
    }
}