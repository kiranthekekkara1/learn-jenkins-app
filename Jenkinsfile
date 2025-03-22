pipeline {
    agent any


    options {
        cleanBeforeCheckout()  // Ensures workspace cleanup only before fetching code
    }

    stages {

        stage('Setup Go') {
            steps {
                script {
                    def root = tool(name: '1.13', type: 'go')  // ✅ Correct syntax
                    env.PATH = "${root}/bin:${env.PATH}"      // Add Go to PATH
                    sh 'go version'  // Verify Go installation
                    sh 'which go'
                    sh 'echo $WORKSPACE'
                    sh 'ls -ltr'
                } // Closing script block ✅
            } // Closing steps block ✅
        } // Closing stage block ✅
    } // Closing stages block ✅
} // Closing pipeline block ✅