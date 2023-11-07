pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the Git repository
                checkout([$class: 'GitSCM', branches: [[name: 'main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/sj3cmu/spring-petclinic.git']]])
            }
        }

        stage('Build') {
            steps {
                // Build the project (you may need to adjust the build command)
                sh './mvnw package -DskipTests' // Replace with your build command
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
