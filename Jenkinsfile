pipeline {
    agent none

    stages {
        stage('primero') {
            agent any
            steps {
                echo "$GIT_BRANCH"
            }
        }

        stage('build') {
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
            steps {
                container('docker') {
                    sh 'docker compose build'
                }
            }
        }
    }
}