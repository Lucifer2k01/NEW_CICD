# NEW_CICD
node {
    
    stage('Git checkout'){
        git branch: 'main', url: 'https://github.com/Lucifer2k01/NEW_CICD.git'
    }
    stage('sending files to ansible'){
        sshagent(['ansible-demo']) {
            sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.95.93'
            sh 'scp /var/lib/jenkins/workspace/complete_cicd/* ubuntu@172.31.95.93:/home/ubuntu/'
      }
    }
    stage('Docker build image'){
        sshagent(['ansible-demo']){
            sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.95.93 cd /home/ubuntu/'
            sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.95.93 "docker image build -t $JOB_NAME:v1.$BUILD_ID ."'
        }
        
    }
    
}