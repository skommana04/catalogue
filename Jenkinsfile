pipeline {
    agent{
        node 'slave1'
    }
    // parameters{
    //     choice(name: 'ENV', choices: ['dev', 'test', 'prod'], description: 'Deploy_to')
    // }
    // environment{
    //     IMAGE_TAG = env.GIT_COMMIT
    //  }
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
                    expression { env.GIT_BRANCH.startsWith('f')}
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
            when{
                allOf{
                    expression { env.GIT_BRANCH.startsWith('f')}
                }
            }
            steps{
                script{
                    sh """
                        npm test                     
                    """
                }
            }
        }
        stage('Build Image') {
            when{
                allOf{
                    expression { env.GIT_BRANCH.startsWith('f')}
                }
            }
            steps{
                script{
                    sh """
                        echo "Build Image"
                     
                    """
                }
            }
        }
        stage('Image Scan') {
            when{
                allOf{
                    expression { env.GIT_BRANCH.startsWith('f')}
                }
            }
            steps{
                script{
                    sh """
                        echo "Image Scan"
                     
                    """
                }
            }
        }
        stage('Push Image') {
            when{
                allOf{
                    expression { env.GIT_BRANCH.startsWith('f')}
                }
            }
            steps{
                script{
                    sh """
                        echo "push image"         
                    """
                }
            }
        }
        stage('Approval to Deploy') {
            when{
                allOf{
                    expression { env.GIT_BRANCH.startsWith('f')}
                }
            }
            steps{
                script{
                    sh """
                        echo "push image"         
                    """
                }
            }
        }
        stage('Trigger Deploy') {
            when{
                allOf{
                    expression { env.GIT_BRANCH.startsWith('f')}
                }
            }
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