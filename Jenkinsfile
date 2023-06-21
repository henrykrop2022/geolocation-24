pipeline {
    triggers{
        pollSCM('* * * * *')
    }
    agent {
        docker{ image 'maven:3.8.6-openjdk-18'}
    }
    tools{
        maven 'M2_HOME'
    }
    stages{
        stage('maven build'){
            steps{
                sh 'mvn clean install package'
               }
            }
        }
        stage('Build & SonarQube analysis'){
            steps{
               withSonarQubeEnv('SonarServer'){
                sh 'mvn verify org.sonarsource.scanner.maven:sonar-maven-plugin:sonar -Dsonar.projectKey=henrykrop2022_geolocation-24'
               }
            }
        }
        stage('Check Quality Gate'){
            steps{
                echo 'check quality gate...'
                script{
                    timeout(time: 20, unit:'MINUTES'){
                        def qg = waitForQaulityGate()
                        if (qg.stattus != 'OK'){
                            error "Pipeline stopped because of quality gate status: ${qg.status}"
                        }
                    }
                }
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