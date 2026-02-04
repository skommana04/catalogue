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
                script{
                    /* Use sh """ when you want to use Groovy variables inside the shell.
                    Use sh ''' when you want the script to be treated as pure shell. */
                    sh '''
                    echo "Fetching Dependabot alerts..."

                    response=$(curl -s \
                        -H "Authorization: token ${GITHUB_TOKEN}" \
                        -H "Accept: application/vnd.github+json" \
                        "${GITHUB_API}/repos/${GITHUB_OWNER}/${GITHUB_REPO}/dependabot/alerts?per_page=100")

                    echo "${response}" > dependabot_alerts.json

                    high_critical_open_count=$(echo "${response}" | jq '[.[] 
                        | select(
                            .state == "open"
                            and (.security_advisory.severity == "high"
                                or .security_advisory.severity == "critical")
                        )
                    ] | length')

                    echo "Open HIGH/CRITICAL Dependabot alerts: ${high_critical_open_count}"

                    if [ "${high_critical_open_count}" -gt 0 ]; then
                        echo "❌ Blocking pipeline due to OPEN HIGH/CRITICAL Dependabot alerts"
                        echo "Affected dependencies:"
                        echo "$response" | jq '.[] 
                        | select(.state=="open" 
                        and (.security_advisory.severity=="high" 
                        or .security_advisory.severity=="critical"))
                        | {dependency: .dependency.package.name, severity: .security_advisory.severity, advisory: .security_advisory.summary}'
                        exit 1
                    else
                        echo "✅ No OPEN HIGH/CRITICAL Dependabot alerts found"
                    fi
                    ''' 
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
        stage('Dependabot Security Gate'){
            environment {
                GITHUB_OWNER = 'skommana04'
                GITHUB_REPO = 'catalogue'
                GITHUB_API = 'https://api.github.com'
                GITHUB_TOKEN = credentials('GITHUB_TOKEN')
            }
            steps {
                script{
                    sh '''
                    echo "Fetching Dependabot alerts..."
                    response = $(curl -s \
                        -H "Authorization: Bearer ${GITHUB_TOKEN}" \
                        -H "Accept: application/vnd.github+json" \
                        "${GITHUB_API}/repos/${GITHUB_OWNER}/${GITHUB_REPO}/dependabot/alerts \
                        | jq length)

                        echo "${response}: \$ALERT_COUNT"

                        if [ "\$ALERT_COUNT" -gt 0 ]; then
                            echo "❌ Dependabot alerts found. Failing pipeline."
                        exit 1
                else
                    echo "✅ No Dependabot alerts found"
                fi











                    """



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
