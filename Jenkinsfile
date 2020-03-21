    def notifySlack(String buildStatus = 'STARTED') {
    // Build status of null means success.
    buildStatus = buildStatus ?: 'SUCCESS'
    def color
    if (buildStatus == 'STARTED') {
        color = '#D4DADF'
    } else if (buildStatus == 'SUCCESS') {
        color = '#BDFFC3'
    } else if (buildStatus == 'UNSTABLE') {
        color = '#FFFE89'
    } else {
        color = '#FF9FA1'
    }
    def msg = "${buildStatus}: `${env.JOB_NAME}` #${env.BUILD_NUMBER}:\n${env.BUILD_URL}"
    slackSend(color: color, message: msg)
}
node('centos-docker') {
stage('check-out-code') {
      checkout([$class: 'GitSCM', branches: [[name: "${branch}"]], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[credentialsId: 'git-creds', url: 'https://github.com/lidorg-dev/final-project.git']]])
}
stage('Build') {
   sh label: '', script: "docker build -t sarah-app:${env.BUILD_ID} ."
}
stage('Test') {
}
stage('Publish') {
   withDockerRegistry(credentialsId: 'docker-hub') {
  sh label: '', script: "docker tag sarah-app:${env.BUILD_ID} lidorlg/sarah-app:${env.BUILD_ID} && docker push lidorlg/sarah-app:${env.BUILD_ID}"
}
}
stage('Notify Slack') {
  try {
       notifySlack()
       // Existing build steps.
   } catch (e) {
       currentBuild.result = 'FAILURE'
       throw e
   } finally {
       notifySlack(currentBuild.result)
   }
}

