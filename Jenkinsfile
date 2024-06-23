pipeline{
    agent any
    
    environment{
        PATH = "/opt/maven3/bin:$PATH"
    }
    stages{
        stage("Git Checkout"){
            steps{
                git credentialsId: 'springboot', url: 'https://github.com/yelurusrinu/springboot.git'
            }
        }
        stage("Maven Build"){
            steps{
                sh "mvn clean package"
                sh "mv target/*.war target/myweb-war.war"
            }
        }
        stage("deploy-dev"){
            steps{
                sshagent(['tomcat-new']) {
                sh """
                    scp -o StrictHostKeyChecking=no target/myweb-war.war  ec2-user@172.31.41.210:/home/ubuntu/apache-tomcat-9.0.89/webapps/
                    
                    ssh ec2-user@172.31.41.210 /home/ubuntu/apache-tomcat-9.0.89/bin/shutdown.sh
                    
                    ssh ec2-user@172.31.41.210 /home/ubuntu/apache-tomcat-9.0.89/bin/startup.sh
                
                """
            }
            
            }
        }
    }
}
