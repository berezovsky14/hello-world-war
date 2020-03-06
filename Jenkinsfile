pipeline {
   
    agent any
    stages{
        stage('Build'){
            steps { 
                sh 'mvn clean package'
                 
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts :  '**/target/*.war'
                }
            }
        }
        stage('SonarQube Analysis') {
            def mvnHome =  tool name: 'maven-3', type: 'maven'
            withSonarQubeEnv('sonar-6') { 
              sh "${mvnHome}/bin/mvn sonar:sonar"
        }
    }
    }
}
