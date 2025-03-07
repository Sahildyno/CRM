pipeline {
    agent any

    environment {
        JAVA_HOME = "/usr/lib/jvm/java-11-openjdk-amd64"
        NODE_VERSION = "18.17.0" // Use a stable Node.js version
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'develop', url: 'https://github.com/Sahildyno/CRM.git'
            }
        }

        stage('Install Node.js and Dependencies') {
            steps {
                script {
                    if (!fileExists("$HOME/node-v${NODE_VERSION}-linux-x64/bin/node")) {
                        echo "Downloading and installing Node.js v${NODE_VERSION}..."
                        sh '''
                        curl -o node.tar.xz https://nodejs.org/dist/v${NODE_VERSION}/node-v${NODE_VERSION}-linux-x64.tar.xz
                        tar -xf node.tar.xz
                        mv node-v${NODE_VERSION}-linux-x64 $HOME/
                        rm node.tar.xz
                        '''
                    }
                    // Set Node.js in PATH
                    env.PATH = "$HOME/node-v${NODE_VERSION}-linux-x64/bin:$PATH"
                    sh 'node -v && npm -v' // Verify Node.js installation
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    if (fileExists('package.json')) {
                        sh 'npm install'
                    } else {
                        error "No package.json found!"
                    }
                }
            }
        }

        stage('Build Application') {
            steps {
                script {
                    if (fileExists('package.json')) {
                        sh 'npm run build'
                    }
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    if (fileExists('package.json')) {
                        sh 'npm test'
                    }
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application...'
                // Deployment steps (e.g., Docker, Kubernetes, SCP, AWS)
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
