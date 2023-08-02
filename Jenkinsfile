pipeline {
    agent any
    options {
        skipDefaultCheckout(true)
    }
    stages {
        stage('SCM') {
            steps {
                checkout scm
            }
        }
        stage('SonarQube Analysis') {
            steps {
                def scannerHome = tool 'SonarScanner';
                withSonarQubeEnv(SonarQube) {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage('Approval for Terraform') {
            steps {
                input(message: 'Approval required before Terraform', ok: 'Proceed', submitterParameter: 'APPROVER')
            }
        }
        stage('terraform') {
            steps {
                sh '/usr/local/bin/docker run --rm -v "$(pwd):/src" aquasec/tfsec .'
                // Replace the above 'docker run' command with your actual Terraform command
                // e.g., sh '/usr/local/bin/docker run --rm -v "$(pwd):/src" your-terraform-image your-terraform-command'
            }
        }
    }
    post {
        always {
            cleanWs()
        }
    }
}
