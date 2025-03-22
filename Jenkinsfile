pipeline {
    environment{
      GOPATH = "$WORKSPACE"
    }
    agent any

    stages {
        stage(' Test and Build ') {
            steps {
               script{
                  def root = tool(name: '1.13', type: 'go')  // ✅ Corrected syntax
                  env.PATH = "${root}/bin:${env.PATH}"      // Add Go to PATH
                  sh 'go version'  // Verify Go installation
                  sh "echo $GOPATH"
                  sh "which go"
            }
        }
    }
}
