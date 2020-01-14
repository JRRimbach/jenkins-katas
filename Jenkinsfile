pipeline {
  agent any
  stages {
    stage('Say Hello') {
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
          steps {
            sh 'ci/build-app.sh'
          }
        }

      }
    }

    stage('build app') {
      agent {
        docker {
          image 'gradle:jdk11'
        }

      }
      steps {
        sh 'ci/build-app.sh'
      }
    }

  }
}