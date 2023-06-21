pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    stages{
        stage('maven build'){
            steps{
                sh 'mvn clean install package'
            }
        }
        stage('Build & SonarQube analysis'){
            agent any
            steps{
                withSonarQubeEnv('sonarQube')
                sh 'mvn sonar:sonar'
            }
        }
        stage('Upload artifact'){
            steps{
                script{
                    def mavenPom = readMavenPom file: 'pom.xml'
                nexusArtifactUploader artifacts: 
                [[artifactId: "${mavenPom.artifactId}", 
                classifier: '', 
                file: "target/${mavenPom.artifactId}-${mavenPom.version}.${mavenPom.packaging}", 
                type: "${mavenPom.packaging}"]], 
                credentialsId: 'nexusID',
                 groupId: "${mavenPom.groupId}", 
                 nexusUrl: '192.168.56.32:8081', 
                 nexusVersion: 'nexus3', 
                 protocol: 'http', 
                 repository: 'maven-snapshots', 
                 version: "${mavenPom.version}"
                }
            }
        }
    }
}