pipeline {
    agent any
    
    tools {
	maven 'localMaven'	
	}
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '34.230.72.136', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.222.204.165', description: 'Production Server')
    } 

    triggers {
         pollSCM('* * * * *') // Polling Source Control
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
			echo '$JENKINS_HOME/jobs//jobs//branches//builds/$BUILD_NUMBER/archive/'
			echo "${params.tomcat_dev}"
			echo 'echo y | pscp -i C:\\tomcat_new-demo.pem C:\\Users\\Administrator\\.jenkins\\jobs\\FullyAutomate\\builds\\44\\archive\\webapp\\target\\*.war'
			    timeout(time: 2, unit: 'MINUTES')
				{

				     bat 'echo y | pscp -i C:\\tomcat_new-demo.pem C:\\Users\\Administrator\\.jenkins\\jobs\\FullyAutomate\\builds\\47\\archive\\webapp\\target\\*.war ec2-user@34.230.72.136:/var/lib/tomcat7/webapps'
				}                      
                    }
                }

                stage ("Deploy to Production"){
                    steps {
			    timeout(time: 2, unit: 'MINUTES')
				{
                        		bat 'echo y | pscp -i C:\\tomcat_new-demo-pro.pem C:\\Users\\Administrator\\.jenkins\\jobs\\FullyAutomate\\builds\\48\\archive\\webapp\\target\\*.war ec2-user@18.222.204.165:/var/lib/tomcat7/webapps'
				}
			}
                }
            }
        }
    }
}
