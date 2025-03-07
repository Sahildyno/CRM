pipeline {
    agent any

    environment {
        // Define environment variables if needed
        JAVA_HOME = "/usr/lib/jvm/java-11-openjdk-amd64"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'develop', url: 'https://github.com/Sahildyno/CRM.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    if (fileExists('pom.xml')) {
                        sh 'mvn clean install'
                    } else if (fileExists('package.json')) {
                        sh 'npm install'
                    } else if (fileExists('requirements.txt')) {
                        sh 'pip install -r requirements.txt'
                    } else {
                        error "No recognized dependency file found!"
                    }
                }
            }
        }

        stage('Build Application') {
            steps {
                script {
                    if (fileExists('pom.xml')) {
                        sh 'mvn package'
                    } else if (fileExists('package.json')) {
                        sh 'npm run build'
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    if (fileExists('pom.xml')) {
                        sh 'mvn test'
                    } else if (fileExists('package.json')) {
                        sh 'npm test'
                    } else if (fileExists('pytest.ini')) {
                        sh 'pytest'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                // Add deployment steps (e.g., Docker, Kubernetes, SCP, AWS)
            }
        }
    }

    post {
        success {
            echo 'Build and deployment successful!'
        }
        failure {
            echo 'Build failed!'
        }
    }
}
