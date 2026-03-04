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
                withSonarQubeEnv('SONAR') {
                    sh 'mvn package sonar:sonar'
                }
            }
        }

    }
}