pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('petclinic_analysis')
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the Git repository
                checkout([$class: 'GitSCM', branches: [[name: 'main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/sj3cmu/spring-petclinic.git']]])
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

        stage('Build') {
            steps {
                // Build the project (you may need to adjust the build command)
                sh 'mvn clean package' // Replace with your build command
            }
        }

        stage('Execute petclinic.jar') {
            steps {
                script {
                    // Execute the petclinic.jar (update the path as needed)
                    sh 'java -jar target/petclinic.jar' // Replace with your Java command
                }
            }
        }
    }

    post {
        success {
            // Capture a screenshot of the welcome screen (you may need to install a headless browser tool)
            sh 'screenshot-command -o welcome-screen.png' // Replace with the actual screenshot command
            archiveArtifacts artifacts: 'welcome-screen.png', allowEmptyArchive: true
        }
        failure {
            // Actions to perform on failure (you can add your custom actions here)
            echo 'The pipeline failed!'
        }
    }
}
