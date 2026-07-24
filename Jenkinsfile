pipeline {

    agent any

    tools {
        maven 'M3'
    }

    stages {

        stage('Build & Test') {

            steps {

                sh 'mvn -B verify'

            }

        }

        stage('Archive Jar') {

            steps {

                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true

            }

        }

    }

    post {

        always {

            junit 'target/surefire-reports/*.xml'

        }

        success {

            echo 'Pipeline Successful'

        }

        failure {

            echo 'Pipeline Failed'

        }

    }

}