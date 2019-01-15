pipeline {
    agent any

    tools {
        maven 'LocalMaven'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: 'ec2-18-191-99-89.us-east-2.compute.amazonaws.com', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: 'ec2-18-217-120-12.us-east-2.compute.amazonaws.com', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
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
                        bat "echo y | pscp -i C:\\Putty\\tomcat-demo.ppk .\\webapp\\target\\*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat8/webapps"
                    }
                }
 
                stage ("Deploy to Production"){
                    steps {
                        bat "echo y | pscp -i C:\\Putty\\tomcat-demo.ppk .\\webapp\\target\\*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}