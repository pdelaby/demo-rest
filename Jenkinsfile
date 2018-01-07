pipeline {
    agent any
   
    environment {   
	  def apacheProps = readProperties  file:'/var/lib/jenkins/jobconf/apache.properties'
	  def publicHtmlPath = "${apacheProps['apache.docpath']}"
    }
	
    stages { 
                        
        stage('Checkout'){
            steps{
                checkout scm
            }
        }
        
        stage('Install'){
            steps{
                sh "npm install"
            }			
        }		
           		
		stage('Build') {
			steps{				
				sh "npm run buildprod "
			}
		}
        
        stage('Deploy'){
            steps{
                sh "mkdir -p ${publicHtmlPath}/demo-rest-front"
                 sh "cp -R dist/* ${publicHtmlPath}/demo-rest-front"
            }
        }
        
    }
    
    
    post{
        always{
         
			// TOUJOURS TOUJORUS nettoyer le workspace
			cleanWs()
        }
    }
}