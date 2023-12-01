pipeline {
    agent any
    
    stages {
        stage('Clear Docker images') {
            steps {
                script {
                    sh 'docker stop $(docker ps -qa)'
                    sh 'docker rm $(docker ps -qa)'
                    sh 'docker rmi $(docker images -q)'
                }
            }
        }
        stage('Build and run Docker-container') {
            steps {
                script {
                    sh 'docker build -f Dockerfile -t api.py .'
                    sh 'docker run -d -p 5000:5000 api.py:latest'
                }
            }
        }
        stage('Create dir reports, venv, download trivy template, install semgrep') {
            steps {
                script {
                    sh '''#!/bin/bash 
                    mkdir reports
                    curl -sfL https://raw.githubusercontent.com/aquasecurity/trivy/main/contrib/html.tpl > html.tpl
                    python -m venv venv
                    source ./venv/bin/activate 
                    pip install semgrep
                    deactivate'''
                }
            }
        }
        stage('Security Scan with Trivy') {
            steps {
                script {
                    sh 'trivy image --ignore-unfixed -f template --template "@html.tpl" -o ./reports/trivy-report.html --scanners vuln api.py:latest'
                    publishHTML target : [
                        allowMissing: true,
                        alwaysLinkToLastBuild: true,
                        keepAll: true,
                        reportDir: 'reports',
                        reportFiles: 'trivy-report.html',
                        reportName: 'Trivy Scan Vulns',
                        reportTitles: 'Trivy Scan Vulns'
                    ]
                    sh 'trivy image --ignore-unfixed --exit-code 1 --severity HIGH,CRITICAL --security-checks vuln api.py:latest'
                }
            }
        }
        stage('Security Scan with Semgrep') {
            steps {
                script {
                    sh './venv/bin/semgrep --config auto --junit-xml -o ./reports/semgrep-report.xml ./api.py'
                    junit skipMarkingBuildUnstable: true, testResults: 'reports/semgrep-report.xml'
                }
            }
        }
    }
}
