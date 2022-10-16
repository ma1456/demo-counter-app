pipeline {
    agent any

    stages {
        stage ('git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ma1456/demo-counter-app.git'
            }
        }
        stage ( 'Unit Testing') {

            steps{
                sh 'mvn test'
            }
        }
        stage ( 'Integration Testing') {
            steps{
                sh 'mvn verify -DskipUnitTests'
            }
        }
        stage ( 'Maven Build') {
            steps{
                sh 'mvn clean install'
            }
        }
        stage ( 'SonarQube Analysis') {
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'SonarQube') {
                     sh ' mvn clean package sonar:sonar'
                    } 
                }
            }
        }
        stage ( 'Quality Gate Status'){
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'SonarQube'
                }
            }
        }
        stage ( 'Nexus Repository'){
            steps{
                script{
                    def readPomVersion =readMavenPom file: 'pom.xml'

                    def nexusRepo = readMavenPom.version.endswith( "SNAPSHOT") ? "manoj-demo-app" : "manoj-app-demo"
                    
                    nexusArtifactUploader artifacts:
                     [
                         [
                             artifactId: 'springboot',
                              classifier: '',
                               file: 'target/Uber.jar',
                                type: 'jar'
                                ]
                                ],
                                 credentialsId: 'manoj-nexus',
                                  groupId: 'com.example',
                                   nexusUrl: '3.135.227.143:8081',
                                    nexusVersion: 'nexus3',
                                     protocol: 'http',
                                      repository: 'http://3.135.227.143:8081/repository/manoj-app-demo/',
                                       version: "${readPomVersion.version}"
                }
            }
        }
        
    }
}