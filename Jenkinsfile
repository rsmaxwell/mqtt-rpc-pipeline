pipeline {
  agent {
    kubernetes {
      yamlFile 'KubernetesPod.yaml'
    }
  }

  environment {
    FAMILY = 'linux'
    ARCHITECTURE = 'amd64'
  }


  stages {

    stage('prepare') {
      steps {
        container('tools') {
          dir('project') {
            echo "preparing the application (FAMILY=${env.FAMILY}, ARCH=${env.ARCHITECTURE})"
            checkout([
              $class: 'GitSCM', 
              branches: [[name: '*/main']], 
              extensions: [], 
              userRemoteConfigs: [[url: 'https://github.com/rsmaxwell/mqtt-rpc']]
            ])
            sh('./scripts/prepare.sh')
          }
        }
      }
    }

    stage('deploy') {
      steps {
        container('gradle') {
          dir('project') {
            echo 'deploying the application'
            sh('./scripts/deploy.sh')
          }
        }
      }
    }
  }
}
