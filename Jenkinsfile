pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: 'main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/sj3cmu/spring-petclinic.git']]])
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarQube'
                    def scannerScript = "${scannerHome}/bin/sonar-scanner"
                    withSonarQubeEnv('SonarQube') {
                    sh "${scannerScript}"
                    }
                }
            }
        }

        stage('Build') {
            steps {
                sh './mvnw package -DskipTests' 
            }
        }
    }

    post {
        success {
           echo 'The pipeline successful!'
        }
        failure {
            echo 'The pipeline failed!'
        }
    }
}
