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
        
    }
}