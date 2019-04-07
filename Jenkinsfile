pipeline {
  agent any

  parameters {
    string(name: 'CHORT_VERSION', defaultValue: '', description: 'デプロイするバージョン')
    string(name: 'SUBRA_BRANCH', defaultValue: 'master', description: 'Chefのブランチ')
  }

  stages {
    stage('Deploy') {
      steps {
        ws("${env.WORKSPACE}/../chef") {
          script {
            git url: 'https://github.com/Leonis0813/subra.git', branch: params.SUBRA_BRANCH

            def version = params.CHORT_VERSION.replaceFirst(/^.+\//, '')
            sh "sudo CHORT_VERSION=${version} chef-client -z -r chort"
          }
        }
      }
    }
  }
}
