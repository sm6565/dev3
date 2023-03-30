pipeline {
  agent any
  tools { 
        maven 'Maven_3_6_3'  
    }
   stages{
    
    stage('Run SAST') {
        steps {	
    sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=smdemo -Dsonar.organization=setth -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=f6fc24f42103facc3402fdd5a39b0ccec8ea0444'
        }
}  
    stage('SCA by Prisma') {
            steps {
            withCredentials([string(credentialsId: 'PCC_USER', variable: 'USER'), string(credentialsId: 'PCC_PASS', variable: 'PASS')]) 
            {
                script {
                 docker.image('bridgecrew/checkov:latest').inside("--entrypoint=''") {
                        
                            sh 'checkov --quiet --soft-fail -d . --use-enforcement-rules -o cli --bc-api-key "$USER"::"$PASS" --prisma-api-url https://api.jp.prismacloud.io '
                          
                          }
                }          
            }    
            }  
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
            steps {
          withCredentials([string(credentialsId: 'PCC_CONSOLE_URL', variable: 'CONSOLE'), string(credentialsId: 'PCC_PASS', variable: 'PASS'), string(credentialsId: 'PCC_USER', variable: 'USER')]) {
   
      sh ''' curl -k -u "$USER":"$PASS" --output ./twistcli $CONSOLE/api/v1/util/twistcli
            chmod a+x ./twistcli
            ./twistcli images scan --dockerAddress unix:///var/run/docker.sock --address "$CONSOLE" --user "$USER"  --password "$PASS" --details webserver '''  
     } } 
           } 

    stage('Push to Jfrog') {
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
