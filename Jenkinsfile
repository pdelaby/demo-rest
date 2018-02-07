pipeline {
    agent any
   
   environment{
       // PREREQUIS : Jenkins a un scanner sonar installé qui s'apelle sonarqube-scanner
       def sonarqubeScannerHome = tool name: 'sonarqube-scanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
   }
    stages { 
	
		stage('Env') {
		
		  steps{
			// PREREQUIS : Jenkins a un fichier JSON apache-conf avec un object publichtml.root qui contient la valeur souhaitée
			configFileProvider([configFile(fileId:'apache-conf', variable: 'apacheConfFile')]) {
			  script{
				def apacheConf = readJSON(text: readFile(file: apacheConfFile))
				env.publicApacheRoot = "$apacheConf.publichtml.root"
			  }
			}
			
			// PREREQUIS : Jenkins a un fichier JSON sonar-conf avec un object sonar.url qui contient la valeur souhaitée
			configFileProvider([configFile(fileId:'sonar-conf', variable: 'sonarConfFile')]) {
			  script{
				def sonarConf = readJSON(text: readFile(file: sonarConfFile))
				env.sonarUrl = "$sonarConf.sonar.url"
			  }
			}
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
		
		stage('sonar'){
		    steps{
				// utilisation du scanner de jenkins 
				// avec le fichier de conf sonar du projet
				// et l'url configurée par jenkins
		        sh "${sonarqubeScannerHome}/bin/sonar-scanner -Dsonar.host.url=${sonarUrl} -Dproject.settings=sonar.properties"
		    }
		}
        
        stage('Deploy'){
            steps{
                sh "mkdir -p ${publicApacheRoot}/demo-rest-front"
                 sh "cp -R dist/* ${publicApacheRoot}/demo-rest-front"
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