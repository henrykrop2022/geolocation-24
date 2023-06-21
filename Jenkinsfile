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
    stage{
        stages('Maven Build'){
            steps{
                sh 'mvn clean install package'
            }
        }
    }
}