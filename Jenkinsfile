pipeline{
    triggers{
        pollSCM('* * * * *')
    }
    agent{
        docker{ image 'maven:3.8.6-openjdk-18'}
    }
    tools{
        maven 'M2_HOME'
    }
    stages{
        stage("Maven Build"){
            steps{
                sh 'mvn clean install package'  
            }
         }
         stage("Build & SonarQube analysis"){
            steps{
                withSonarQubeEnv('SonarQube'){
                    sh 'maven:sonar-maven-plugin:sonar -Dsonar.projectKey=henrykrop2022_geolocation-24'
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
    }
}