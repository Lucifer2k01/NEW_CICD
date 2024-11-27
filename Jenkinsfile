pipeline {
    agent any
    environment {
        name = 'arvind'
        docker_password = 'arvind@123'
    }
    stages { 
        stage('Git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/Lucifer2k01/NEW_CICD.git'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') { 
                    sh "sonar-scanner \
                        -Dsonar.projectKey=NEW_CICD \
                        -Dsonar.sources=. \
                        -Dsonar.host.url=http://your-sonarqube-url \
                        -Dsonar.login=your-sonarqube-token"
                }
            }
        }
        stage('Docker build image') {
            steps {
                sh 'docker image build . -t myhtmlpage'
                sh 'docker image tag myhtmlpage arvind2k01/myhtmlpage'
            }
        }
        stage('Docker image push') {
            steps {
                sh 'docker login -u arvind2k01 -p $docker_password'
                sh 'docker image push arvind2k01/myhtmlpage'
            }
        }
        stage('Docker file run') {
            steps {
                sh 'docker pull arvind2k01/myhtmlpage'
                sh 'docker run -d -p 80:80 arvind2k01/myhtmlpage'
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                sh 'ansible-playbook -i inventory playbook.yaml'
            }
        }
    }
}
