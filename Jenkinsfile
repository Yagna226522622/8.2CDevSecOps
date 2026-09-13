pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Yagna226522622/8.2CDevSecOps.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm test || true'
            }
        }

        stage('Generate Coverage Report') {
            steps {
                sh 'npm run coverage || true'
            }
        }

        stage('NPM Audit (Security Scan)') {
            steps {
                sh 'npm audit || true'
            }
        }

        stage('SonarCloud Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    withCredentials([
                        string(
                            credentialsId: 'sonarcloud-token',
                            variable: 'SONAR_TOKEN'
                        )
                    ]) {
                        script {
                            def scannerHome = tool 'SonarScanner'

                            sh """
                                ${scannerHome}/bin/sonar-scanner \
                                -Dsonar.token=\$SONAR_TOKEN
                            """
                        }
                    }
                }
            }
        }
    }
}