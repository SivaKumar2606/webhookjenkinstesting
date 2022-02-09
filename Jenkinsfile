pipeline {
    agent any
    stages {
         stage("Clone Repo") {
            steps { 
                sh "rm -rf webhookjenkinstesting || true"
                sh "git clone https://github.com/SivaKumar2606/webhookjenkinstesting.git"
                sh "cp /var/lib/jenkins/workspace/pipeline1/webhookjenkinstesting/* /var/lib/jenkins/workspace/pipeline1/"
            } 
         }
         stage("Removing Existing Image & Container") {
            steps {
                sh "docker -H tcp://172.16.1.6:2375 stop jk-mywebpage || true"
                sh "docker -H tcp://172.16.1.6:2375 rmi -f sivakumar2606/pipeline1:v1 || true"                
            }
         }   
         stage("Building New DockerImage") {
            steps {
                sh "cd /var/lib/jenkins/workspace/pipeline1/"
                sh "docker rmi -f sivakumar2606/pipeline1:v1 || true"
                sh "sleep 3s"
                sh "docker build -t sivakumar2606/pipeline1:v1 ."
            }
         }
         stage("Pushing the New Image to hub Registry") {
            steps {
               sh "docker push sivakumar2606/pipeline1:v1"
            }   
        }
         stage("Deploy Container in Remote Node") {
            steps {
              sh "docker -H tcp://172.16.1.6:2375 run --rm -dit --name jk-mywebpage --hostname jk-mywebpage -p 9000:80 sivakumar2606/pipeline1:v1"
            }  
        }
        stage("Check Reachability") {
            steps {
              sh "sleep 5s"
              sh "curl http://13.68.218.161:9000"
            }
       }     
    }
}
