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
        stage('Test'){
            steps {
                echo "Testing"
            }
        }
        stage('Deploy') {
            // input {
            //     message "continue?"
            //     ok "yes"
            //     submitter "medha"
            //     parameters {
            //         string(name: 'PERSON', defaultValue: 'Mr.Jenkins', description: 'who should i say')
            //     }
            // }
            when {
                expression { "$params.DEPLOY" == "true" }

            }
            steps{
                echo "Depying"
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
