pipeline {
    agent any

    stages {
        stage('primero') {
            steps {
                echo "$GIT_BRANCH"
            }

        }
    
        stage ('build') {
            steps {
                sh(script: 'docker compose build')
            }
        }
    }
}