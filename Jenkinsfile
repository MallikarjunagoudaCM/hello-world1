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
                withCredentials([string(credentialsId: 'ac71f888-b05d-4b62-9154-6d9cfe08ad38', variable: 'dockercredn')]) {

                sh "docker build --tag mallikarjunagouda/webapp:v1 ."
                  sh "docker push -u mallikarjunagouda -p ${dockercredn} mallikarjunagouda/webapp:v1"
                }                
               
            }
        }


        stage ('Docker Image build on Digital_node') {
            steps {
                sshagent(['a6a793f6-2b73-4107-bc3b-39ce4461d106']) {
                  git 'https://github.com/MallikarjunagoudaCM/hello-world1.git'
                  sh "docker build --tag mallikarjunagouda/webapp:v1 ."
                  sh "docker push mallikarjunagouda/webapp:v1"
                }
               
            }
        }
                   
        
        
    }
    
}
