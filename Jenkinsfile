pipeline {
    agent any

    stages {
        stage('Code Checkout Stage') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: 'main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/sj3cmu/spring-petclinic.git']]])
            }
        }

        stage('SonarQube Static Analysis Stage') {
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

        stage('Build Stage') {
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
