pipeline {
    agent any

    environment {
      NETLIFY_SITE_ID = '966fdfed-d7b0-471a-827a-ac43efbba35c'
    }

    stages {
        stage('Build') {
          agent {
            docker {
              image 'node:18-alpine'
              reuseNode true
            }
          }
          steps {
            sh '''
              ls -la
              node --version
              npm --version
              npm install
              npm run build
              ls -la
            '''
          }
        }

        stage ('Test') {
          agent {
            docker {
              image 'node:18-alpine'
              reuseNode true
            }
          }
          steps {
            sh '''
              npm test
            '''
          }
        }

        stage ('Deploy') {
          agent {
            docker {
              image 'node:18-alpine'
              reuseNode true
            }
          }
          steps {
            sh '''
              npm install netlify-cli
              ./node_modules/.bin/netlify --version
              echo "Deploying to prod with SITE ID: $NETLIFY_SITE_ID"
            '''
          }
        }
    }

    post {
      always {
        junit 'test-results/junit.xml'
      }
    }
}