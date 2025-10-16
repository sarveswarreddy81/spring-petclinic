
pipeline {
    agent {
        label 'JAVAJDKSPC'
    }

    stages {
        stage('GIT CHECKOUT') {
            steps {
                git branch: 'main', url: 'https://github.com/sarveswarreddy81/spring-petclinic.git'
            }
        }

        stage('Build and Scan') {
            steps {
                withCredentials([string(credentialsId: 'sonar_id1', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv('SONARQUBE') {
                        sh '''
                            mvn clean package sonar:sonar \
                              -Dsonar.host.url=https://sonarcloud.io \
                              -Dsonar.organization=sarveswarreddy81 \
                              -Dsonar.projectName=spring-petclinic \
                              -Dsonar.projectKey=sarveswarreddy81_spring-petclinic \
                              -Dsonar.login=$SONAR_TOKEN
                        '''
                    }
                }
            }
        }

        stage('Upload to JFrog') {
            steps {
                rtUpload (
                    serverId: 'JFROG_MAIN',
                    spec: '''{
                      "files": [
                        {
                          "pattern": "target/*.jar",
                          "target": "mavenrepo-libs-release/"
                        }
                      ]
                    }'''
                )
                rtPublishBuildInfo (
                    serverId: 'JFROG_JAVA'
                )
            }
        }
    }
}
