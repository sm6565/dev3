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
    stage('Checkout') {
          steps {
              git branch: 'main', url: 'https://github.com/sm6565/dev2.git'
              stash includes: '**/*', name: 'source'
          } 
    }         
    stage('SCA by Checkov') {
            steps {
                withCredentials([string(credentialsId: 'PCC_CONSOLE_URL', variable: 'CONSOLE'), string(credentialsId: 'PCC_PASS', variable: 'PASS'), string(credentialsId: 'PCC_USER', variable: 'USER')])
                 {
                    
                        script { 
                          docker.image('bridgecrew/checkov:latest').inside("--entrypoint=''") {
                          unstash 'source'
                        
                              sh 'export PRISMA_API_URL=https://api.prismacloud.io'
                              sh 'checkov --quiet -d . --use-enforcement-rules -o cli --bc-api-key c0a0e49b-d9ac-4e92-99f2-e42f044ec7c5::0c8VXZBBM57RawBl6DdNooQrHN8= --prisma-api-url https://api.prismacloud.io '
                          
                          }
                        }
                    
                }
            }    
        }
   }
}
