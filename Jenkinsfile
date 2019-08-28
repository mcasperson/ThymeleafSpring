pipeline {
    agent any
    tools {
        maven 'Maven 3.5.2'
        jdk 'Java 9'
    }
    stages {
        stage ('Initialize') {
            steps {
                sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }

        stage ('Build') {
            steps {
                sh 'mvn package'
            }
        }

        stage ('Deploy to Octopus') {
            steps {
                withCredentials([string(credentialsId: 'OctopusAPIKey', variable: 'APIKey')]) {
                    sh """
                        ${tool('Octo CLI')}/Octo push --package target/demo.0.0.1-SNAPSHOT.war --replace-existing --server https://java-certs-version-change.octopushq.com --apiKey ${APIKey}
                        ${tool('Octo CLI')}/Octo create-release --project "Thymeleaf Demo" --server https://java-certs-version-change.octopushq.com --apiKey ${APIKey}
                        ${tool('Octo CLI')}/Octo deploy-release --project "Thymeleaf Demo" --version latest --deployto Integration --server https://java-certs-version-change.octopushq.com --apiKey ${APIKey}
                    """
                }
            }
        }
    }
}