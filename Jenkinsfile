
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Validate Files') {
            steps {
                script {
                    if (!fileExists('index.html')) {
                        error('index.html is missing')
                    }

                    if (!fileExists('Dockerfile')) {
                        error('Dockerfile is missing')
                    }

                    if (!fileExists('Jenkinsfile')) {
                        error('Jenkinsfile is missing')
                    }
                }

                echo 'All required application files are present.'
            }
        }

        stage('Test Dashboard') {
            steps {
                script {
                    def html = readFile('index.html')

                    if (!html.contains('Intersection Monitoring')) {
                        error('Intersection monitoring test failed')
                    }

                    if (!html.contains('Waiting time exceeded 120 seconds')) {
                        error('Waiting-time alert test failed')
                    }

                    if (!html.contains('Central Junction')) {
                        error('Traffic intersection data test failed')
                    }

                    if (!html.contains('Traffic Diversion and Advisories')) {
                        error('Traffic diversion test failed')
                    }
                }

                echo 'Dashboard content validation passed.'
            }
        }

       stage('Build Docker Image') {
    steps {
        bat 'docker build -t assessment-10 .'
    }
}
    }

    post {
        success {
            echo 'Traffic management pipeline completed successfully.'
        }

        failure {
            echo 'Pipeline failed. Check the console output for the error.'
        }
    }
}

