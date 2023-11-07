pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('petclinic_analysis')
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    // Check out the source code from your repository
                    checkout scm
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build the project (you may need to adjust the build command)
                    sh 'mvn clean package'  // Replace with your build command
                }
            }
        }

        stage('Static Analysis') {
            steps {
                withSonarQubeEnv('SonarQube Server') {
                    // Run static code analysis using SonarQube
                    script {
                        def scannerHome = tool name: 'SonarQubeScanner', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.host.url=http://localhost:9000 -Dsonar.login=${env.SONAR_TOKEN}"
                    }
                }
            }
        }

        stage('Execute petclinic.jar') {
            steps {
                script {
                    // Execute the petclinic.jar (update the path as needed)
                    sh 'java -jar /path/to/petclinic.jar'
                }
            }
        }
    }

    post {
        success {
            // Notify or perform actions on success
        }
        failure {
            // Notify or perform actions on failure
        }
    }
}
