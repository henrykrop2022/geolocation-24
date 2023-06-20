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
                nexusArtifactUploader artifacts: 
                [[artifactId: '${POM_ARTIFACTID}', 
                classifier: '', 
                file: 'target/${POM_ARTIFACTID}-${POM_VERSION}.${POM_PACKAGING}', 
                type: '${POM_PACKAGING}']], 
                credentialsId: 'nexusID',
                 groupId: '${POM_GROUPID}', 
                 nexusUrl: '192.168.56.32:8081', 
                 nexusVersion: 'nexus3', 
                 protocol: 'http', 
                 repository: 'maven-snapshots', 
                 version: '${POM_VERSION}'
                }
            }
        }
    }
}