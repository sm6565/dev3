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
                 app =  docker.build("webapp1")
                 }
               }
            }
    }
  stage('Scan Image') { 
            steps {
          withCredentials([string(credentialsId: 'PCC_CONSOLE_URL', variable: 'CONSOLE'), string(credentialsId: 'PCC_PASS', variable: 'PASS'), string(credentialsId: 'PCC_USER', variable: 'USER')]) {
   
      sh ''' curl -k -u "$USER":"$PASS" --output ./twistcli $CONSOLE/api/v1/util/twistcli
            chmod a+x ./twistcli
            ./twistcli images scan --dockerAddress unix:///var/run/docker.sock --address "$CONSOLE" --user "$USER"  --password "$PASS" --details webapp1 ''' 
      
     } } 
           }  

	/* stage('Push') {
            steps {
                script{
                    docker.withRegistry('https://572392880480.dkr.ecr.us-east-1.amazonaws.com/webapp1', 'ecr:us-east-1:aws-credentials') {
                    app.push("latest")
                    }
                }
            }
    	} */
	    
  }
}
