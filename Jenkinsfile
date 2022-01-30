pipeline {
    agent { label 'Master' }
    
    stages {
        
        stage ('Git') {
            
            steps {
                git 'https://github.com/MallikarjunagoudaCM/hello-world1.git'
            }
            
        }
        
        stage ('build') {
            environment { PATH= "/opt/apache-maven-3.8.4/bin:$PATH" }
            steps {
               sh "mvn clean package"
               sh "pwd"
            }
        }
        
        
        
    }
    
}
