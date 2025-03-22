pipeline {
    agent any

    options {
        skipDefaultCheckout(true)  // ✅ Prevents Jenkins from auto-checking out the code
    }

    stages {
    stage('Clean Workspace') {
            steps {
                script {
                    deleteDir()  // ✅ Ensures workspace cleanup before fetching code
                }
                 checkout scm 
            }
        }
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