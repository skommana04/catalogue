pipeline {
    agent{
        node {
            label 'slave1'
        }
    }
    environment{
        ACC_ID = "654654431182"
        GITHUB_REPO = "catalogue"
        GITHUB_PROJECT = "roboshop"
    }
    stages{
        stage('Read Version'){
            steps{
                script{
                    def packageJson = readJSON file: 'package.json'
                    env.appVersion = packageJson.version
                    echo "app Version: ${env.appVersion}"
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
                GITHUB_API = "https://api.github.com"
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
        stage('Build Image'){
            steps{     
                script{
                    //withCredentials([string(credentialsId: 'aws-creds', variable: 'AWS_CREDS')]) {  
                    withAWS(credentials: 'aws-creds', region: 'us-east-1') {               
                    sh """
                        echo "building image"
                        echo "login to ecr"
                        aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com

                        docker build -t ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${GITHUB_PROJECT}/${GITHUB_REPO}:${env.appVersion} .
                        docker images
                        docker push ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${GITHUB_PROJECT}/${GITHUB_REPO}:${env.appVersion}
                    """
                    }
                }
            }
        }
        stage('Trivy Scan'){
            steps{
                script{
                    sh """
                        echo "scanning the image with Trivy"
                        trivy image --scanners vuln --pkg-types os --severity CRITICAL,HIGH,MEDIUM --exit-code 1 --format table ${ACC_ID}.dkr.ecr.us-east-1.amazonaws.com/${GITHUB_PROJECT}/${GITHUB_REPO}:${env.appVersion}
                    """
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