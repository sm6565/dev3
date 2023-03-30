pipeline {
  agent any
  tools { 
        maven 'Maven_3_6_3'  
    }
   stages{
  stage('SCA by Prisma') {
            /*steps {
            withCredentials([string(credentialsId: 'PCC_USER', variable: 'USER'), string(credentialsId: 'PCC_PASS', variable: 'PASS')]) 
            {
                script {
                 docker.image('bridgecrew/checkov:latest').inside("--entrypoint=''") {
                        
                            sh 'checkov --quiet --soft-fail -d . --use-enforcement-rules -o cli --bc-api-key "$USER"::"$PASS" --prisma-api-url https://api.prismacloud.io '
                          
                          }
                }          
            }    
            } */ 
    }
	stage('Build') { 
            steps { 
               withDockerRegistry([credentialsId: "dockerlogin", url: ""]) {
                 script{
                 app =  docker.build("webserver")
                 }
               }
            }
    }
  stage('Scan Image') { 
            /*steps {
          withCredentials([string(credentialsId: 'PCC_CONSOLE_URL', variable: 'CONSOLE'), string(credentialsId: 'PCC_PASS', variable: 'PASS'), string(credentialsId: 'PCC_USER', variable: 'USER')]) {
   
      sh ''' curl -k -u "$USER":"$PASS" --output ./twistcli $CONSOLE/api/v1/util/twistcli
            chmod a+x ./twistcli
            ./twistcli images scan --dockerAddress unix:///var/run/docker.sock --address "$CONSOLE" --user "$USER"  --password "$PASS" --details webserver ''' 
      
     } } */
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
