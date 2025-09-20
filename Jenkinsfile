pipeline {
    agent any
    stages{
        stage('Checkout stage'){
            steps{
                git url:'https://github.com/gyanpunj/php-project/', branch: "master"
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t kubergyan1/php-project:v2 .'
                    sh 'docker images'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-cred', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push kubergyan1/php-project:v2'
                }
            }
        }
        
     stage('Deploy') {
            steps {
               script {
                   def dockerrm = 'sudo docker rm -f My-first-containe2211 || true'
                    def dockerCmd = 'sudo docker run -itd --name My-first-containe2211 -p 8083:80 kubergyan1/php-project:v2'
                    sshagent(['ubuntu']) {
                        //chnage the private ip in below code
                        // sh "docker run -itd --name My-first-containe2111 -p 8083:80 kubergyan1/php-project:v2"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.172 ${dockerrm}"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.38.172 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
