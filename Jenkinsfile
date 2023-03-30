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
        sh ''' docker login -u setthapong4u@gmail.com sett1.jfrog.io -p cmVmdGtuOjAxOjE3MTEzNzIxMDU6NkFJQ0g2MXdRd2JMM0tKbXZsN1RiN2JtSGU1
             docker tag webserver sett1.jfrog.io/app1/webserver:latest
             docker push sett1.jfrog.io/app1/webserver:latest '''
                  
            }
    	} 
	    
  }
}
