pipeline {
    agent any

    tools {
        maven 'Maven 3.8.6' // Podesi verziju Maven-a u Jenkins-u
        jdk 'JDK 17'        // Podesi verziju JDK-a u Jenkins-u
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '3'))
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package -DskipTests'
            }
        }

        stage('Deploy') {
            steps {
                sh 'echo "Deploying application..."'
                sh 'cp target/basic-java-app-1.0-SNAPSHOT.jar /tmp/'
            }
        }
    }


    post {
        always {
            sh 'echo "Cleaning up..."'
        }

        success {
            echo 'Build succeeded!'
        }

        failure {
            echo 'Build failed!'
        }
    }
}
