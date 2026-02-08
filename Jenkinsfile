pipeline {
    agent{
        node 'slave1'
    }
    parameters{
        choice(name: 'ENV', choices: ['dev', 'test', 'prod'], description: 'Deploy_to')
    }
    stages {
        stage('GET COMMIT ID'){
            script{
                env.IMAGE_TAG = env.GIT_COMMIT
            }
        }
        stage('Build the code'){
            when{
                allof{
                    expression { env.GIT_BRANCH.startswith('feature/')}
                    expression { params.ENV == 'dev' }
                }
            }
            steps{
                script{
                    sh """
                        echo "hi"
                    """
                }
            }
        }
        stage('unit tests') {
            steps{
                script{
                    sh """
                        echo "Unit Tests"
                     
                    """
                }
            }
        }
        stage('Build Image') {
            steps{
                script{
                    sh """
                        echo "Build Image"
                     
                    """
                }
            }
        }
        stage('Image Scan') {
            steps{
                script{
                    sh """
                        echo "Image Scan"
                     
                    """
                }
            }
        }
        stage('Push Image') {
            steps{
                script{
                    sh """
                        echo "push image"         
                    """
                }
            }
        }
        stage('Approval to Deploy') {
            steps{
                script{
                    sh """
                        echo "push image"         
                    """
                }
            }
        }
        stage('Trigger Deploy') {
            steps{
                script{
                    sh """
                        echo "Trigger Deploy"         
                    """
                }
            }
        }
    }
    post{
        always{
            echo "I will say Hello"
        }
    }

}