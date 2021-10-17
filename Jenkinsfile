pipeline {
  agent any 

  stages {
    stage('Daily Compliance Run') {
      steps{
        echo 'Running a compliance scan with inspec....'
          script{
            def remote = [:]
            remote.name = "node-1"
            remote.host = "188.166.208.199"
            remote.allowAnyHosts = true

            withCredentials([sshUserPrivateKey(credentialsId: 'sshUser', keyFileVariable: 'identity', passphraseVariable: '', usernameVariable: 'userName')]) {
                remote.user = userName
                remote.identityFile = identity
                stage("Enforce with Ansible") {
                  sshCommand remote: remote, command: 'cd /root/ansible/ansible-bootcamp-code/chap10/ && git pull origin'
                  sshCommand remote: remote, command: 'cd /root/ansible/ansible-bootcamp-code/chap10/ && ansible-playbook compliance.yaml'
              }
                stage("Scan with InSpec") {
                  sshCommand remote: remote, command: 'inspec exec --no-distinct-exit /root/linux-baseline/'
              }
            }
          }
       }
    }
  }
}

