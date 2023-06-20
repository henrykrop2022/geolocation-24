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
        stage('Upload artifact'){
            steps{
                script{
                    def mavenPom = readMaven file: 'pom.xml'
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