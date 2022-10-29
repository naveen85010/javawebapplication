
pipeline {
    agent {
        node {
            label 'node1'
        }
    }
    
   stages{

   stage("git check out") {
    git credentialsId: 'github', url: 'https://github.com/naveen85010/javawebapplication.git'

   }

   stage("maven build"){
    steps{
        script{
            def mvnHome = tool name: 'maven-3', type: 'maven'
            sh "${mvnHome}/bin/mvn package"
        }
        
    }



   }
   }
    
    
    }
    
