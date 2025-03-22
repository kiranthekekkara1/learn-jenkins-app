pipeline {
    environment{
      GOPATH = "$WORKSPACE"
    }
    agent any

    stages {
        stage(' Test and Build ') {
            steps {
               script{
                def root = tool name: '1.13' type: 'go'
                withEnv("GOPATH=${GOPATH}/go"){
                  sh '''
                    echo $GOPATH
                    echo $WORKSPACE
                    go version
                  '''
                }
               }
            }
        }
    }
}
