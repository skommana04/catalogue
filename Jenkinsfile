pipeline {
    agent{
        node {
            label 'slave1'
        }
    }

    stages{
        stage('Build Code'){
            steps{
                script{
                    sh '''
                        npm install
                    '''
                }
            }
        }
        stage('Unit Test'){
            steps{
                script{
                    sh '''
                        npm test
                    '''
                }
            }
        }
    }

    post{
        always{
            echo "pipeline finished"
        }
    }



}