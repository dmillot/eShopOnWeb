pipeline {
  agent any
  stages {
    stage('Build') {
      steps {
        sh 'dotnet build eShopOnWeb.sln'
      }
    }

    stage('Tests') {
      parallel {
        stage('Unit') {
          steps {
            sh 'dotnet test tests/UnitTests'
          }
        }

        stage('Integration') {
          steps {
            sh 'dotnet test tests/IntegrationTests'
          }
        }

        stage('Functional') {
          steps {
            warnError(message: 'Functional problem') {
              sh 'dotnet test tests/FunctionalTests'
            }

          }
        }

      }
    }

    stage('Publish') {
      steps {
        sh 'dotnet publish eShopOnWeb.sln -o /var/jenkins_home/aspnet'
        dir(path: '/var/jenkins_home/aspnet') {
          archiveArtifacts(artifacts: '*', onlyIfSuccessful: true)
        }

      }
    }

  }
}