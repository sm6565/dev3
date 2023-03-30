pipeline {
    agent any
    tools { 
          maven 'Maven_3_6_3'  
      }
     stages{
    
      stage('Build') { 
              steps { 
                 withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                   script{
                   app =  docker.build("webserver")
                   }
                 }
              }
      }
  
      stage('Push') {
              steps {
                withCredentials([string(credentialsId: 'J_USER', variable: 'JUSER'), string(credentialsId: 'J_PASS', variable: 'JPASS')]) 
           {
          sh ''' docker login -u "$JUSER" sett1.jfrog.io -p "$JPASS"
               docker tag webserver sett1.jfrog.io/app1/webserver:latest
               docker push sett1.jfrog.io/app1/webserver:latest '''
                    
              } }
          } 
          
    }
  }
