pipeline{
    triggers{
        pollSCM('* * * * *')
    }
    // agent{
    //     docker{ image 'maven:3.8.6-openjdk-18'}
    // }
    agent any
    tools{
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
                sh 'mvn package -DskipTests'
            }
         }
        // 
        stage('SonarQube analysis') {
    environment {
        scannerHome = tool 'sonarQube' // the name you have given the Sonar Scanner (Global Tool Configuration)
    }
    steps {
        withSonarQubeEnv(installationName: 'SonarQube') {
            sh 'mvn sonar:sonar'
        }
    }
}
          stage('Check Quality Gate') {
            steps {
                echo 'Checking quality gate...'
                 script {
                     timeout(time: 20, unit: 'MINUTES') {
                         def qg = waitForQualityGate()
                         if (qg.status != 'OK') {
                             error "Pipeline stopped because of quality gate status: ${qg.status}"
                         }
                     }
                 }
            }
        }
         stage('Build Image') {
            
            steps {
                script{
                  def mavenPom = readMavenPom file: 'pom.xml'
                    dockerImage = docker.build registry + ":${mavenPom.version}"
                } 
            }
        }
        stage('Deploy image') {
           
            
            steps{
                script{ 
                    docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }    
    }
}