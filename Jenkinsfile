lswci([node: 'docker']) {
  withDockerRegistry([url: "https://artifactory.devleaseweb.com", credentialsId: 'svc_jenkins']) {
    stage('Build image') {
      sshagent(["jenkins-ci-key"]) {
        def tag = sh(script: 'git tag --points-at HEAD', returnStdout: true).trim()
        if (tag.length() == 0) tag = 'latest'

        def dockerApp = docker.build("artifactory.devleaseweb.com/openstack-docker/openstack-exporter:${tag}")
        if (!dockerApp) {
            throw new Exception('Docker build image failed')
        }
        if (env.BRANCH_NAME == 'main') {
          dockerApp.push(tag)
        }
      }
    }
  }
}
