
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
            withSonarQubeEnv('sonar-6') { 
              sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
        }
    }
    }
