pipeline {
    agent {
        node {
            label 'slave1'
        }
    }
    stages {
        stage ('build') {
                steps{
                    script {
                        sh """
                            echo "Hi darling"
                        """
                    }
                }
        } 
    }
}