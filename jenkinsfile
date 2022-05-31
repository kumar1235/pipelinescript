pipeline{
    agent any
    environment{
        PATH= "/opt/maven/bin:$PATH"
    }
    stages{
	    stage("Git Checkout"){
            steps{
                git 'https://github.com/javahometech/myweb'
            }
       }
       
        stage("Build"){
            steps{
                sh "mvn clean package"
                sh "mv target/*.war target/myweb.war"
            }
            
        }
        
        stage("Deploy"){
            steps{
              sshagent(['tomcat_new']) {
                sh """
                 scp -o StrictHostKeyChecking=no webapp/target/webapp.war ec2-user@172.31.18.49:/opt/tomcat/webapps/
                 ssh ec2-user@172.31.18.49 /opt/tomcat/bin/shutdown.sh
                 ssh ec2-user@172.31.18.49 /opt/tomcat/bin/startup.sh
                """
              }
            }
        }    
    }
    
}
