pipeline {
    agent { label 'Master' }
    parameters {
        choice(choices: ['v1', 'v2', 'v3', 'v4'], description: 'Choose version of Docker Image', name: 'version')
    }
    
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
                 
                sh "docker build --tag mallikarjunagouda/webapp:${params.version} ."
                sh "docker login -u mallikarjunagouda -p ${dockercred}"
                sh "docker push mallikarjunagouda/webapp:${params.version}"
                sh "docker rmi mallikarjunagouda/webapp:${params.version}"
                }                
               
            }
        }


        stage ('deploy into Docker Container') {
            environment { dockerDel = "docker rm -f web" 
                          dockerRun = "docker run -d -p 8080:8080 --name web mallikarjunagouda/webapp:${params.version}" }
            steps {
                sshagent(['a6a793f6-2b73-4107-bc3b-39ce4461d106']) {
                    sh "ssh -o StrictHostKeyChecking=no root@159.89.170.18 ${dockerDel}"
                    sh "ssh -o StrictHostKeyChecking=no root@159.89.170.18 ${dockerRun}"                    
                }
               
            }
        }
                   
        
        
    }
    
}
