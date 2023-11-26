pipeline {
    agent jenkins

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

        stage('Deploy with Ansible') {
            steps {
                script{
                    // Define the extra variables
                    def extraVars = "workspace=${WORKSPACE}"
                    // Build the ansible-playbook command
                    def ansibleCmd = "ansible-playbook -i inventory.yml ${WORKSPACE}/deploy_petclinic.yml --extra-vars '${extraVars}'"
                    // Execute the command
                    sh ansibleCmd
                }
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
