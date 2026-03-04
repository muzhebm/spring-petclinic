pipeline {
    agent { label 'spc'}
    triggers {
        pollSCM('* * * * *')
    }
    stages{
        stage("git checkout"){
            steps{
                git url :'https://github.com/muzhebm/spring-petclinic.git',
                    branch: 'main'
            }
        }
        stage('build and scan'){
            steps{
                withCredential([string(credentialsId: 'sonar_id',variable: 'SONAR_TOKEN')]){
                withSonarQubeEnv('SONAR') {
                    sh """mvn package sonar:sonar \
                          -Dsonar.projectKey=muzhebm_spring-petclinic \
                          -Dsonar.organization=muzhebm \
                          -Dsonar.host.url=https://sonarcloud.io/ \
                          -Dsoanr.login=$SONAR_TOKEN"""
                }
                }
                
                }
            }
        }

    }
}