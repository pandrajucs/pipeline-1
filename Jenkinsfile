pipeline{
agent any
stages{
stage('terraform init & apply '){
steps{
sh "terraform init"
sh "terraform apply --auto-approve"
    }
  }
  stage('Clone Repo') {
          steps {
            sh 'rm -rf Drum-Kit'
            sh 'git clone https://github.com/panrajucs/Drum-Kit.git /var/lib/jenkins/'
            }
        }
   stage('Build Docker Image') {
          steps {
           
              echo "${PWD}"
            sh 'docker build -t padrajucs/drumkit:${BUILD_NUMBER} .'
            }
        }
       stage('Push Image to Docker Hub') {
          steps {
           sh    'docker push padrajucs/drumkit:${BUILD_NUMBER}'
           }
        }
      stage('Deploy to Docker Host') {
          steps {
            sh    'docker -H tcp://10.1.1.200:2375 stop prodwebapp1 || true'
            sh    'docker -H tcp://10.1.1.200:2375 run --rm -dit --name prodwebapp1 --hostname prodwebapp1 -p 8000:80 padrajucs/drumkit:${BUILD_NUMBER}'
            }
        }

        stage('Check WebApp Rechability') {
          steps {
          sh 'sleep 10s'
          sh ' curl ec2-35-168-62-217.compute-1.amazonaws.com:8000'
          }
        }
 }
}

