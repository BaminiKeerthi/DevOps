pipeline {
    agent any

    tools {
        maven 'Maven 3.8.8'  // Configure this name in Manage Jenkins → Tools
        jdk 'Java 17'        // Configure this name in Manage Jenkins → Tools
    }

    environment {
        DEPLOY_SERVER = "ubuntu@your-server-ip"
        DEPLOY_PATH = "/opt/app"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/YourUser/YourRepo.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                // Example: Copy artifact to deployment server
                sh """
                scp target/*.jar $DEPLOY_SERVER:$DEPLOY_PATH/
                ssh $DEPLOY_SERVER 'pkill -f myapp || true && nohup java -jar $DEPLOY_PATH/*.jar > app.log 2>&1 &'
                """
            }
        }
    }

    post {
        success {
            echo '✅ Build & Deployment successful!'
        }
        failure {
            echo '❌ Build failed. Check logs.'
        }
    }
}
