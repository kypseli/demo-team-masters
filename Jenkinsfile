library 'kypseli@master'
pipeline {
  agent { label 'default-jnlp' }
  options { 
    buildDiscarder(logRotator(numToKeepStr: '5'))
    preserveStashes(buildCount: 5)
  }
  stages {
    stage('Create Team Managed Masters') {
      steps {
        echo "preparing Jenkins CLI"
        sh 'curl -O http://cjoc/cjoc/jnlpJars/jenkins-cli.jar'
        withCredentials([usernamePassword(credentialsId: 'cli-username-token', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          script {
            def teams = ['dev', 'sec']
            teams.each { item ->
              sh """
                alias cli='java -jar jenkins-cli.jar -s \'http://cjoc/cjoc/\' -auth $USERNAME:$PASSWORD'
                cli teams ${item} --put < ${item}-team.json
              """
            }
          }
        }
      }
    }
  }
}
