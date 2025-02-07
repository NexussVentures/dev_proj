pipeline {
    agent any

    tools{
        maven 'maven'
    }

    stages{
        stage('Check and remove container'){
            steps{
                script{
                    def containerExists = sh(script: "docker ps -q -f name=sak", returnStdout: true).trim()
                    if (containerExists) {
                    sh "docker stop sak"
                    sh "docker rm sak"
                    }
                }
            }
        }
        stage('Build package'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Create image'){
            steps{
                sh 'sudo docker build -t app /var/lib/jenkins/workspace/sak/'
            }
        }
        stage('Assign tag'){
            steps{
                sh 'docker tag app sarfarazalikhanma/devops_prac'
            }
        }
        stage('Push to dockerhub'){
            steps{
                sh 'echo "SakHack123." | docker login -u "sarfarazalikhanma" --password-stdin'
                sh 'docker push sarfarazalikhanma/devops_prac'
            }
        }
        stage('Remove images'){
            steps{
                sh 'docker rmi -f $(docker images -q)'
            }
        }
        stage('Pull image from DockerHub'){
            steps{
                sh 'docker pull sarfarazalikhanma/devops_prac'
            }
        }
        stage('Run a container'){
            steps{
                sh 'docker run -it -d --name sak -p 8081:8080 sarfarazalikhanma/devops_prac'
            }
        }
    }
    post {
        success {
            echo 'Deployment successful'
        }
        failure {
            sh 'docker rm -f sak'
        }
        always{
            echo 'Deployed'
        }
    }

}