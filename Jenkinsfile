pipeline {
    agent{
        node {
            label 'slave1'
        }
    }
    stages{
        stage('Read Version'){
            steps{
                script{
                    def packageJson = readJSON file: 'package.json'
                    appVersion = packageJson.version
                    echo "app Version: ${appVersion}"
                }
            }
        }
        stage('Install Dependencies'){
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
        stage('Dependency checks'){
            environment{
                GITHUB_API = "https://github.com"
                GITHUB_OWNER = "skommana04"
                GITHUB_REPO = "catalogue"
                GITHUB_TOKEN = credentials('GITHUB_TOKEN')
            }
            steps{
                script{
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

    }

    post{
        always{
            echo "pipeline finished"
        }
    }



}