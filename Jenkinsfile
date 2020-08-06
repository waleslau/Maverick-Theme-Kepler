pipeline {
  agent any
  stages {
    stage('检出') {
      steps {
        checkout([
          $class: 'GitSCM',
          branches: [[name: env.GIT_BUILD_REF]],
          userRemoteConfigs: [[
            url: env.GIT_REPO_URL,
            credentialsId: env.CREDENTIALS_ID
          ]]])
        }
      }
    stage("Build") {
            steps {
              //  sh 'apt install tree -y && tree'
                sh 'node -v'
                sh 'npm install gulp-cli -g'
                sh 'npm install gulp -D'
                sh 'make'
            }
        }
    stage("Deploy to latest branch") {
            steps {
                sh 'git clone ${REPO_URL} ./REPO \
                    && cd ./REPO \
                    && git checkout latest'
                sh 'rm -rf ./REPO/* \
                    && mv ./Kepler/* ./REPO'
                sh 'cd ./REPO \
                    && git config user.name ${USER_NAME} \
                    && git config user.email ${USER_EMAIL} \
                    && git add . && git commit -m ${GIT_COMMIT} \
                    && git push '
            }
        }
    }
  }