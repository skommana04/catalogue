pipeline {
    agent {
        node {
            label 'slave1'
        }

    }
    
    options {
        // timeout(time: 10, unit: 'MINUTES')
        disableConcurrentBuilds()
    }
    environment {
        COURSE = "Jenkins"
        ACC_ID = "654654431182"
        PROJECT = "roboshop"
	    COMPONENT = "catalogue"

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
        stage('Unit Testing'){
            steps{
                script {
                    sh """
                        npm test
                    """
                }
            }
        }
        // here you need to select scanner tool and send to sonar server
        stage ('Sonar Scan') {
            environment {
                def scannerHome = tool 'sonar-8.0'
            }            
            steps{
                script{                    
                    withSonarQubeEnv('sonar-server') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        // stage ('Quality Gate') {
        //     steps{
        //         timeout(time: 2, unit: 'MINUTE') {
        //         waitForQualityGate abortPipeline: true

        //         }
        //     }
        // }
        // stage ('Build Image'){
        //     steps {
        //         script {
        //             withAWS(region:'us-east-1',credentials:'aws-creds') {
        //                 sh """
        //                     aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com
        //                     docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion} .
        //                     docker images
        //                     docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${PROJECT}/${COMPONENT}:${appVersion}
        //                 """
        //             } 
        //         }

        //     }
        // }
        
       

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
