pipeline {
    agent any
    environment {
        SONAR_TOKEN = credentials('petclinic_analysis')
    }

    stages {
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

        stage('Build') {
            steps {
                script {
                    // Build the project (you may need to adjust the build command)
                    sh 'mvn clean package' // Replace with your build command
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
            // Actions to perform on success (you can add your custom actions here)
            echo 'The pipeline was successful!'
        }
        failure {
            // Actions to perform on failure (you can add your custom actions here)
            echo 'The pipeline failed!'
        }
    }
}
