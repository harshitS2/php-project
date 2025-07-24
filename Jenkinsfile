pipeline {
    agent any

    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://github.com/harshits2/php-project/', branch: 'master'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t harshitpdev/harshitimg24jul:v1 .'
                    sh 'docker images'
                }
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    script {
                        sh 'echo $PASS | docker login -u $USER --password-stdin'
                        sh 'docker push harshitpdev/harshitimg24jul:v1'
                    }
                }
            }
        }

        stage('Deploy on Remote Server') {
            steps {
                script {
                    def dockerRemoveCmd = 'sudo docker rm -f My-first-containe2211 || true'
                    def dockerRunCmd = 'sudo docker run -itd --name My-first-containe2211 -p 8083:80 harshitpdev/harshitimg24jul:v1'

                    sshagent(['sshkeypair']) {
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.17.188 '${dockerRemoveCmd}'"
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.17.188 '${dockerRunCmd}'"
                    }
                }
            }
        }
    }
}
