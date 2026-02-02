pipeline {
    agent {
        node {
            label 'slave1'
        }

    }
    
    options {
        timeout(time: 10, unit: 'MINUTES')
        disableConcurrentBuilds()
    }
    environment {
        COURSE = "Jenkins"
    }


    stages {
        stage ('Read Version'){
            steps {

                script {
                
                        def packageJson = readJSON file: 'package.json'

                        def appVersion = packageJson.version
                        def appName = packageJson.name
                      
                        echo "Application Name: ${appName}"
                        echo "Application Version: ${appVersion}"               
                }
            }
        }
        stage('Install Dependencies'){
            steps {
                script {
                    sh """
                        npm install
                    """
                }
            }
        }
        stage ('Build Image'){
            steps {
                script {
                    withAWS(region:'us-east-1',credentials:'aws-creds') {
                        sh """

                        """
                    } 
                }

            }
        }
        
       

    }
    post {
        always {
            echo "I will always say Hello"
            cleanWs()
        }
        aborted {
            echo "pipeline is aborted"
        }
        success {
            echo "success"
       }
       failure {
        echo "failure"
       }
    }
}
