library 'kypseli@master'
pipeline {
  agent {
    kubernetes {
      label 'create-team-master'
      yaml podYaml
    }
  }
  options { 
    buildDiscarder(logRotator(numToKeepStr: '5'))
    preserveStashes(buildCount: 5)
  }
  stages {
    stage('Create Team Managed Masters') {
      steps {
        echo "preparing Jenkins CLI"
        withCredentials([usernamePassword(credentialsId: 'cli-username-token', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
          script {
            sh """
              alias cli='java -jar jenkins-cli.jar -s \'http://cjoc/cjoc/\' -auth $USERNAME:$PASSWORD'
              def teams = ['dev', 'sec']
              teams.each { item ->
                cli teams ${item} --put < ${item}-team.json
              }
            """
          }
        }
      }
    }
  }
}
