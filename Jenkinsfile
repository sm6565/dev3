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
  stage('Scan Images') { 
            steps {
          
            sh ''' curl -k -u c0a0e49b-d9ac-4e92-99f2-e42f044ec7c5:0c8VXZBBM57RawBl6DdNooQrHN8= --output ./twistcli https://us-east1.cloud.twistlock.com/us-1-111573360/api/v1/util/twistcli
            chmod a+x ./twistcli
             ./twistcli images scan --address https://us-east1.cloud.twistlock.com/us-1-111573360 --user c0a0e49b-d9ac-4e92-99f2-e42f044ec7c5  --password 0c8VXZBBM57RawBl6DdNooQrHN8= --details webapp1 ''' } 
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
