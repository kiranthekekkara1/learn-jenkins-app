pipeline {
    agent any

    stages {
        stage('Setup Go') {
            steps {
                script {
                    def root = tool(name: '1.13', type: 'go')  // ✅ Correct syntax
                    env.PATH = "${root}/bin:${env.PATH}"      // Add Go to PATH
                    sh 'go version'  // Verify Go installation
                } // Closing script block ✅
            } // Closing steps block ✅
        } // Closing stage block ✅
    } // Closing stages block ✅
} // Closing pipeline block ✅