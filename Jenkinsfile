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
    environment{
        ACC_ID = "654654431182"
        GITHUB_REPO = "catalogue"
        GITHUB_PROJECT = "roboshop"
    }
    stages {
        stage('GET COMMIT ID'){
            steps{
                script{  
                    env.IMAGE_TAG = env.GIT_COMMIT.take(4)
                    echo "IMAGE_TAG: ${env.IMAGE_TAG}"            
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
                    //withCredentials([string(credentialsId: 'aws-creds', variable: 'AWS_CREDS')]) {  
                    withAWS(credentials: 'aws-creds', region: 'us-east-1') {               
                    sh """
                        echo "building image"
                        echo "login to ecr"
                        aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com

                        docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${GITHUB_PROJECT}/${GITHUB_REPO}:${env.IMAGE_TAG} .
                        docker images
                        docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${GITHUB_PROJECT}/${GITHUB_REPO}:${env.IMAGE_TAG}
                    """
                    }
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