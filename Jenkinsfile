pipeline {
    agent any

    tools {
        maven 'LocalMaven'
    }

    stages{
        stage('Build'){
            steps {
                cmd 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}