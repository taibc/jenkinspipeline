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
                        bat "scp -i D:\\Du lieu Tai\\nghien cuu\\Jenkins\\Udemy\\Resource\\tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "scp -i D:\\Du lieu Tai\\nghien cuu\\Jenkins\\Udemy\\Resource\\tomcat-demo-virginia.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}
