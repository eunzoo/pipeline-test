pipeline {
  agent {
    kubernetes {
      yaml '''
kind: Pod
metadata:
  name: prReviewTest
spec:
  containers:
  - name: test
    image: registry.acldevsre.de/hello
    imagePullPolicy: Always
    tty: true
  imagePullSecrets:
  - name: pr-review-secret
'''
    }

  }
  stages {
    stage('Image Pull Test') {
      steps {
        container(name: 'test') {
          sh 'echo "Image Pull Succeeded!"'
        }

      }
    }

    stage('Check Vault Crednetial & Git Merge') {
      steps {
        withVault(configuration: [vaultUrl: 'https://dodt-vault.acldevsre.de',  vaultCredentialId: 'approle-for-vault', engineVersion: 2], vaultSecrets: [[path: 'jenkins/eunzoo-public-github', secretValues: [[envVar: 'GITHUB_TOKEN', vaultKey: 'token']]]]) {
          sh "git clone https://${env.GITHUB_TOKEN}@github.com/eunzoo/my-charts.git"
        }

      }
    }

    stage('Send a message') {
      steps {
        mattermostSend(endpoint: 'https://mattermost.acldevsre.de/hooks/1oj1jtrkwi81pe1crj3hwbyohe', icon: 'https://www.jenkins.io/images/logos/automotive/256.png', message: 'Dec. 7, Monday \'20', text: 'This is a test message.')
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
      parallel {
        stage('Add a Jira Comment') {
          steps {
            jiraComment(issueKey: 'EMMA-16', body: 'Dec. 7, Monday : This is a pipeline test comment.')
          }
        }

        stage('JQL Test') {
          steps {
            jiraSearch 'issue=EMMA-16'
          }
        }

      }
    }

  }
}