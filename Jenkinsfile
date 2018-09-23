pipeline {
    agent any
    
    tools {
	maven 'localMaven'	
	}
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '107.21.82.235', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.14.246.243', description: 'Production Server')
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
			echo 'echo y | pscp -i C:\\tomcat_aut_2309.pem C:\\Users\\Administrator\\.jenkins\\jobs\\FullyAutomate\\builds\\44\\archive\\webapp\\target\\*.war'
			    timeout(time: 2, unit: 'MINUTES')
				{

				     bat 'echo y | pscp -i C:\\tomcat_aut_2309.pem C:\\Users\\Administrator\\.jenkins\\jobs\\FullyAutomate\\builds\\51\\archive\\webapp\\target\\*.war ec2-user@ec2-107-21-82-235.compute-1.amazonaws.com:/var/lib/tomcat7/webapps'
				}                      
                    }
                }

                stage ("Deploy to Production"){
                    steps {
			    timeout(time: 2, unit: 'MINUTES')
				{
                        		bat 'echo y | pscp -i C:\\tomcat_pro_2309.pem C:\\Users\\Administrator\\.jenkins\\jobs\\FullyAutomate\\builds\\51\\archive\\webapp\\target\\*.war ec2-user@ec2-52-14-246-243.us-east-2.compute.amazonaws.com:/var/lib/tomcat7/webapps'
				}
			}
                }
            }
        }
    }
}
