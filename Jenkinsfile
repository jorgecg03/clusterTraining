pipeline {
    agent {
        kubernetes {
            yaml """
apiVersion: v1
kind: Pod
spec:
  containers:
  - name: docker
    image: docker:24.0.2-cli
    command:
    - cat
    tty: true
    volumeMounts:
    - name: docker-sock
      mountPath: /var/run/docker.sock
  volumes:
  - name: docker-sock
    hostPath:
      path: /var/run/docker.sock
"""
        }
    }

    stages {
        stage('primero') {

            steps {
                echo "$GIT_BRANCH"
            }
        }

        stage('build') {
            
            steps {
                container('docker') {
                    sh 'docker compose build'
                    sh 'docker ps'
                    sh 'docker images'
                }
            }
        }
        stage('Run app') {
            
            steps {
                container('docker') {
                    sh 'docker compose up -d'
                }
            }
        }
        stage('Run tests') {
            
            steps {
                container('docker') {
                    sh 'pytest ./tests/test_sample.py'
                }
            }
            post {
                success {
                    echo "Tests passed"
                }
                failure {
                    echo "Tests failed"
                }
            }
        }
    }
    

    post {
        always {
            container('docker') {
                sh 'docker compose down'
            }
        }
    }
}

