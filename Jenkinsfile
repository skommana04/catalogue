pipeline {
    agent {
        node {
            label slave1
        }
    }
    stages {
        stage ('build') {
            script {
                sh """
                    echo "Hi darling"
                """
            }

        }
    }
    
}