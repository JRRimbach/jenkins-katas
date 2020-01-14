pipeline {
  agent any
  stages {
    stage('clone down') {
      agent {
        node {
          label 'host'
        }

      }
      steps {
        stash(name: 'code', excludes: '.git')
      }
    }

    stage('Parallel execution') {
      parallel {
        stage('Say Hello') {
          steps {
            sh 'echo "hello world"'
          }
        }

        stage('build app') {
          agent {
            docker {
              image 'gradle:jdk11'
            }

          }
          options {
            skipDefaultCheckout(true)
          }
          steps {
            unstash 'code'
            sh 'ci/build-app.sh'
            archiveArtifacts 'app/build/libs/'
            stash(name: 'code2', excludes: '.git')
          }
        }

      }
    }

    stage('build docker') {
      environment {
        DOCKERCREDS = credentials('docker_login')
      }
      steps {
        unstash 'code2'
        sh 'ci/build-docker.sh'
        sh 'sh \'echo "$DOCKERCREDS_PSW" | docker login -u "$DOCKERCREDS_USR" --password-stdin\' //login to docker '
      }
    }

    stage('push docker') {
      when {
        branch 'master'
      }
      steps {
        sh 'sh \'ci/push-docker.sh\''
      }
    }

  }
}