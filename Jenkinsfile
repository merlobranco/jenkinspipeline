pipeline {
    agent any

    tools {
        maven 'LocalMaven'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.191.99.89', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.217.120.12', description: 'Production Server')
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
                        bat "pscp -i C:\\Putty\\tomcat-demo.ppk **/target/*.war ec2-user@${params.tomcat_dev}:/var/lib/tomcat7/webapps"
                    }
                }
 
                stage ("Deploy to Production"){
                    steps {
                        bat "pscp -i C:\\Putty\\tomcat-demo.ppk **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat7/webapps"
                    }
                }
            }
        }
    }
}