node {
    stage('Checkout source code') {
        checkout scm
    }
    stage('Install packages') {
      sh("yarn install")
      sh("sudo chown -R jenkins: ./node_modules")
    }


    stage('Build static assets') {
      sh("yarn run build")
    }

    stage('Run tests') {
      parallel(
        'Unit Tests': {
            try {
              def TEST_EXIT_CODE = sh(script:"yarn run test --silent", returnStatus: true)
              println TEST_EXIT_CODE
              assert TEST_EXIT_CODE == 0
            } catch (AssertionError e) {
                throw e
            }        
        }
      )
    }
    stage('Deploy') {
      sh("yarn run start")
    }
}