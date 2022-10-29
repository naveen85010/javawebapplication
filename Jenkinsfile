
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
    
    Sh """
      mv target/* target/javawebapplication.war
      scp -o StrictHostKeyChecking=no targer/javawebapplication.war linux-slave@172.31.36.56:/opt/tomcat8/webapps/
      linux-slave@172.31.36.56 /opt/tomcat8/bin/.shutdown.sh
      linux-slave@172.31.36.56 /opt/tomcat8/bin/.startup.sh
    """

      }
       }

   }
       
       
       
       
   }
    
    
    }
    
