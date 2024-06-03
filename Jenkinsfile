pipeline {

    agent any
    tools {
        maven "MAVEN3"
        jdk "OracleJDK8"
    }

    stages {
        stage('Fetch Code') {
            steps {
                git url: 'https://github.com/iamreymond/CI-Jenkins.git', branch: 'Testing_Development'
            }
        }
        stage('Build') {
            steps {
                script {
                    // clean and install the Maven Project
                    sh 'mvn clean install'
                }
            }

            post {
                success {
                    echo 'Now archiving it...'
                    archiveArctifacts artifacts: '**/target/*.war', allowEmptyArchive: true
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    // run maven tests
                    sh 'mvn test'
                }
            }
        }

    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed.'
        }
        always {
            // always do this at the end of the pipeline
            junit 'target/surefire-reports/*.xml'
        }
    }
}