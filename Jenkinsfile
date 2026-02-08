pipeline {
    agent{
        node 'slave1'
    }
    parameters{
        choice(name: 'ENV', choices: ['dev', 'test', 'prod'], description: 'Deploy_to')
    }
    // environment{
    //     IMAGE_TAG = env.GIT_COMMIT
    // }
    stages {
        stage('GET COMMIT ID'){
            steps{
                script{  
                    def IMAGE_TAG = env.GIT_COMMIT.take(4)
                    echo "IMAGE_TAG: ${IMAGE_TAG}"            
                }
            }       
        }
        stage('Install dependencies'){
            when{
                allOf{
                    expression { env.GIT_BRANCH.startsWith('feature/')}
                    //expression { params.ENV == 'dev' }
                }
            }
            steps{
                script{
                    sh """
                        npm install                        
                    """
                }
            }
        }
        stage('unit tests') {
            steps{
                script{
                    sh """
                        npm test                     
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