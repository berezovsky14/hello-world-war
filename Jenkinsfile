
    
        notifyBuild('STARTED')
        stage('1. Checking Out Code'){
            checkout([$class: 'GitSCM', branches: [[name: '*/dev']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'Git-Creds', url: 'https://github.com/SaraBenShabbat/hello-world-war.git']]])
        }
        stage('2. Building'){
            sh label: '', script: "mvn clean package"
        }
        stage('3. Analyzing'){
            sh label: '', script: '''cd /opt/tomcat/.jenkins/workspace/No.6_module-FinalProject
                                      mvn verify sonar:sonar'''
        }
        stage('4. Building Dockerfile'){
            sh label: '', script: '''cp /opt/tomcat/.jenkins/workspace/No.6_module-FinalProject/target/hello-world-war-1.0.0.war /opt/tomcat/.jenkins/workspace/No.6_module-FinalProject
                                     docker build .'''
        }
        stage('5. Tagging Docker Image'){
            sh label: '', script: 'docker tag $(docker images | grep \'<none>\' | head -n 1 | awk \'{print $3}\') java-app:${BUILD_ID}'
        }
        stage('6. Uploading Image To Nexus'){
            withCredentials([usernamePassword(credentialsId: 'Nexus-Docker', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                sh label: '', script: '''docker login -u $USERNAME -p $PASSWORD 192.168.1.233:8082
                                         docker tag java-app:${BUILD_ID} 192.168.1.233:8082/java-app:${BUILD_ID}
                                         docker push 192.168.1.233:8082/java-app:${BUILD_ID}
                                         docker rmi $(docker images --filter=reference="192.168.1.233:8082/java-app*" -q) -f
                                         docker logout 192.168.1.233'''
            }
        
  
        stage('7. Notifying Slack'){
            notifyBuild(currentBuild.result)
        }
    
