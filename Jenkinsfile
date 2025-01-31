pipeline {
    agent any

    tools {
        maven 'Maven 3.8.6' // Ensure this version is configured in Jenkins
        jdk 'JDK 11'        // Ensure JDK version is configured in Jenkins
    }

    environment {
        AWS_REGION = 'us-east-1'  // Change this to your AWS region
        EB_APPLICATION_NAME = 'my-java-web-app' // AWS Beanstalk App Name
        EB_ENVIRONMENT_NAME = 'my-java-web-app-env' // AWS Beanstalk Env Name
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

        stage('Deploy to AWS Elastic Beanstalk') {
            steps {
                withAWS(credentials: 'aws-eb-creds', region: "${AWS_REGION}") {
                    beanstalkDeploy(
                        applicationName: "${EB_APPLICATION_NAME}",
                        environmentName: "${EB_ENVIRONMENT_NAME}",
                        versionLabelFormat: "build-${BUILD_NUMBER}",
                        warFile: 'target/simple-java-web-app-1.0-SNAPSHOT.war'
                    )
                }
            }
        }
    }

    post {
        success {
            echo "Deployment to AWS Elastic Beanstalk succeeded!"
        }

        failure {
            echo "Deployment failed!"
        }
    }
}
