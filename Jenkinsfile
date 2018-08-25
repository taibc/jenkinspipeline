pipeline {
    agent any
    
    tools {
	maven 'localMaven'	
	}
    
    parameters { 
         string(name: 'tomcat_dev', defaultValue: '52.14.28.67', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '52.91.193.134', description: 'Production Server')
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
			echo 'echo y | pscp -i C:\\tomcat-demo.pem C:\\Users\\Administrator\\.jenkins\\jobs\\FullyAutomate\\builds\\34\\archive\\webapp\\target\\*.war'
                        bat 'echo y | pscp -i tC:\\tomcat-demo.pem \'target\*.war\' ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps'
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat 'echo y | pscp -i C:\\tomcat-demo-virginia.pem \'target\*.war\' ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps'
                    }
                }
            }
        }
    }
}
