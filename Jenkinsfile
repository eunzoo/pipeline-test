pipeline {
  agent {
    node {
      label 'jenkins-jenkins-slave'
    }

  }
  stages {
    stage('Send a message') {
      steps {
        mattermostSend(endpoint: 'https://mattermost.acldevsre.de/hooks/e3ry938dj7gw98u9u6ecg6seoy', icon: 'https://www.jenkins.io/images/logos/automotive/256.png', message: 'Nov. 23, Monday \'20', text: 'This is a test message.')
      }
    }

  }
}