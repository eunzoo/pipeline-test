pipeline {
  agent {
    node {
      label 'jenkins-jenkins-slave'
    }

  }
  stages {
    stage('Request an approval') {
      steps {
        input(message: 'Request an approval for sending a message', submitter: 'jmungmoong.roh')
      }
    }

    stage('Send a message') {
      steps {
        mattermostSend(endpoint: 'https://mattermost.acldevsre.de/hooks/e3ry938dj7gw98u9u6ecg6seoy', icon: 'https://www.jenkins.io/images/logos/automotive/256.png', message: 'Nov. 27, Friday \'20', text: 'This is a test message.')
      }
    }

    stage('Check Vault Crednetial & Git Merge') {
      steps {
        withVault(configuration: [vaultUrl: 'https://dodt-vault.acldevsre.de',  vaultCredentialId: 'approle-for-vault', engineVersion: 2], vaultSecrets: [[path: 'jenkins/eunzoo-public-github', secretValues: [[envVar: 'GITHUB_TOKEN', vaultKey: 'token']]]]) {
          sh "git clone https://${env.GITHUB_TOKEN}@github.com/eunzoo/my-charts.git"
        }

      }
    }

    stage('Git merge') {
      steps {
        sh '''ls -al
cd my-charts
git branch -a

#git checkout -b dev origin/merge-test-2
#git branch -a
#git checkout master
#git merge dev
#git push origin master

'''
      }
    }

    stage('Add a Jira Comment') {
      steps {
        jiraComment(issueKey: 'EMMA-15', body: 'Nov. 27, Friday : This is a pipeline test comment.')
      }
    }

  }
}