
pipeline {
    agent {
        node {
            label 'node1'
        }
    }
    
   stages{

   stage("git check out") {
       steps {
    git credentialsId: 'github', url: 'https://github.com/naveen85010/javawebapplication.git'
       }
   }

   stage("maven build"){
    steps{
        script{
            def mvnHome = tool name: 'maven-3', type: 'maven'
            sh "${mvnHome}/bin/mvn package"
        }
        
    }
   }
       
       
       stage("deploy to tomcat"){
       steps{
        sshagent(['tomcat8']) {
    // some block
    
    sh """
      mv target/*.war target/javawebapplication.war
      scp -o StrictHostKeyChecking=no target/javawebapplication.war ec2-user@172.31.38.104:/opt/tomcat8/webapps/
      
      ssh ec2-user@172.31.38.104 /opt/tomcat8/bin/.shutdown.sh
      ssh ec2-user@172.31.38.104 /opt/tomcat8/bin/.startup.sh
    """

      }
       }

   }
       
       
       
       
   }
    
    
    }
    
