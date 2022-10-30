
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
       
        stage("quality check condition"){
    steps{
        script{
            withSonarQubeEnv(credentialsId: 'sonar-token') {
           def mvnHome = tool name: 'maven-3', type: 'maven'
                sh "${mvnHome}/bin/mvn clean sonar:sonar"

           timeout(unit: 'SECONDS', time: 20){
            //def qg = waitForQualityGate()

             //if (qg.status != 'OK'){
               //  error " pipeline got aborted due to quality gate failes ${qg.status}"
             //}
               
               qualitygate = waitForQualityGate()
                    if (qualitygate.status != "OK") {
                        currentBuild.result = "UNSTABLE"
                    }
           }
          }
        }
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
      
      ssh ec2-user@172.31.38.104 /opt/tomcat8/bin/shutdown.sh
      ssh ec2-user@172.31.38.104 /opt/tomcat8/bin/startup.sh
    """

      }
       }

   }
       
       
       
       
   }
    
    
    }
    
