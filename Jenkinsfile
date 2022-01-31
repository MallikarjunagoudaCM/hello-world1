pipeline {
    agent { label 'Master' }
    
    stages {
        
        stage ('Git') {
            
            steps {
                git 'https://github.com/MallikarjunagoudaCM/hello-world1.git'
            }
            
        }
        
        stage ('MVN build') {
            environment { PATH= "/opt/apache-maven-3.8.4/bin:$PATH" }
            steps {
               sh "mvn clean package"
               sh "pwd"
               sh "ls"
            }
        }
        
        stage ('Docker Image build on Master') {
            steps {
                withCredentials([string(credentialsId: 'dockercredn', variable: 'dockercred')]) {
                 sh "whoami"
                sh "docker build --tag mallikarjunagouda/webapp:v1 ."
                sh "docker login -u mallikarjunagouda -p ${dockercred}"
                sh "docker push mallikarjunagouda/webapp:v1"
                }                
               
            }
        }


        stage ('Docker Image build on Digital_node') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'a6a793f6-2b73-4107-bc3b-39ce4461d106', keyFileVariable: '', usernameVariable: 'root')]) {
                  sh 'ssh -o StrictHostKeyChecking=no root@159.89.170.18 uptime'
                  git 'https://github.com/MallikarjunagoudaCM/hello-world1.git'
                  sh "hostname && pwd"
                  sh "docker build --tag mallikarjunagouda/webapp:v1 ."
                  sh "docker push mallikarjunagouda/webapp:v1"
                }
               
            }
        }
                   
        
        
    }
    
}
