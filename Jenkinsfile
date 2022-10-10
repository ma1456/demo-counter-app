pipeline {
    agent any

    stages {
        stage ('git checkout') {
            step {
                git branch: 'main', url: 'https://github.com/ma1456/demo-counter-app.git'
            }
        }
        
    }
}