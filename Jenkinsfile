pipeline {
    agent any

    tools {
        maven 'Maven 3.8.6' // Ensure Maven version is configured in Jenkins
        jdk 'JDK 17'        // Ensure JDK version is configured in Jenkins
    }

    environment {
        AWS_REGION = 'us-east-1'  // AWS region where your Beanstalk app is deployed
        EB_APPLICATION_NAME = 'my-java-web-app' // Name of your Elastic Beanstalk application
        EB_ENVIRONMENT_NAME = 'my-java-web-app-env' // Name of your Elastic Beanstalk environment
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
                    // Deploy WAR file to AWS Elastic Beanstalk
                    sh """
                        mkdir -p .elasticbeanstalk
                        echo 'region: ${AWS_REGION}' > .elasticbeanstalk/config.yml
                        eb init -p java-17 ${EB_APPLICATION_NAME} --region ${AWS_REGION}
                        eb deploy ${EB_ENVIRONMENT_NAME}
                    """
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
