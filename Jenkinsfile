pipeline {
    agent any
    stages{
        stage('git cloned'){
            steps{
                git url:'https://github.com/BALACHANDRAVK/php-project/', branch: "master"
              
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t balachandravk/php:v101 .'
                    sh 'docker images'
                }
            }
        }
          stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push balachandravk/php:v101'
                }
            }
        }
        
     stage('Deploy') {
            steps {
               script {
                   def dockerrm = 'sudo docker rm -f My-first-containe221 || true'
                    def dockerCmd = 'sudo docker run -itd --name My-first-containe221 -p 8082:80 balachandravk/php:v101'
                    sshagent(['sshkey']) {
                        //chnage the private ip in below code
                        // sh "docker run -itd --name My-first-containe211 -p 8082:80 balachandravk/php:v101"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@3.111.35.82 ${dockerrm}"
                         sh "ssh -o StrictHostKeyChecking=no ubuntu@3.111.35.82 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
