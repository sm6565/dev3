pipeline {
  agent any
  tools { 
        maven 'Maven_3_6_3'  
    }
   stages{
    /* stage('CompileandRunSAST') {
            steps {	
		sh 'mvn clean verify sonar:sonar -Dsonar.projectKey=smdemo -Dsonar.organization=setth -Dsonar.host.url=https://sonarcloud.io -Dsonar.login=f6fc24f42103facc3402fdd5a39b0ccec8ea0444'
			}
        }  */
           
       stage('SCA by Prisma Cloud') {
            steps {
                withCredentials([string(credentialsId: 'PCC_CONSOLE_URL', variable: 'CONSOLE'), string(credentialsId: 'PCC_PASS', variable: 'PASS'), string(credentialsId: 'PCC_USER', variable: 'USER')])
                 {
                    
                        script { 
                          docker.image('bridgecrew/checkov:latest').inside("--entrypoint=''") {
                          //unstash 'source'
                        
                              sh 'export PRISMA_API_URL=https://api.prismacloud.io'
			      sh 'echo $PRISMA_API_URL'
			      sh 'echo $USER'
			      sh 'echo "$USER" '
			      sh 'echo "$PCC_USER" '
			      sh 'echo $PCC_USER '	  
                              
			      //sh 'checkov --quiet --soft-fail -d . --use-enforcement-rules -o cli --bc-api-key $PCC_USER::$PCC_PASS --prisma-api-url https://api.prismacloud.io '
                          
                          }
                        }
                    
                }
            }    
        }
   }
}  
