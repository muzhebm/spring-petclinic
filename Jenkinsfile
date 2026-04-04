pipeline {
    agent { label 'spc' }

    triggers {
        pollSCM('* * * * *')
    }

    stages {

        stage("git checkout") {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/muzhebm/spring-petclinic.git'
            }
        }

        stage('build and scan') {
            steps {
                withCredentials([string(credentialsId: 'sonar_id', variable: 'SONAR_TOKEN')]) {

                    withSonarQubeEnv('SONAR') {
                        sh """
                        mvn clean package sonar:sonar \
                        -Dsonar.projectKey=muzhebm_spring-petclinic \
                        -Dsonar.organization=muzhebm \
                        -Dsonar.host.url=https://sonarcloud.io \
                        -Dsonar.login=$SONAR_TOKEN
                        """
                    }

                }
            }
        }

        stage('Binary file store') {
            steps {
                rtupload(
                    ServerId: 'JFROG',
                    Spec: '''{
                        "files":[
                            {
                                "pattern" : "target/*.jar",
                                "target" : "spc-build-info/"
                            }
                        ]
                    }'''
                )

                rtPublishBuildinfo(ServerId: 'JFROG')
            }
        }
    }
}