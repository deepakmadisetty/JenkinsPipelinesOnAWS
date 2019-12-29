pipeline {
  agent any 
  stages {
    stage('Build') {
        steps {
            sh 'echo "Hello World"'
            sh '''
            echo "Multiline shell steps works too"
            ls -lah'''
        }
    }
    stage('Lint HTML') {
        steps {
            sh 'tidy -q -e *.html'
        }
    }
    stage('Upload to AWS') {
        steps {
          timeout(time: 1, unit: 'MINUTES') {
            retry(2) {
              withAWS(region:'us-west-2',credentials:'	jenkins') {
              s3Upload(pathStyleAccessEnabled:true, payloadSigningEnabled: true, file:'index.html', bucket:'udacity-storage-demo')
            }
          }
        }
      }
    }
    stage('Check Availabilty') {
        steps {
          sh 'curl -Is http://udacity-storage-demo.s3-website-us-west-2.amazonaws.com | head -1'
        }
    }
  }
}