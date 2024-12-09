pipeline {
    agent any

    environment {
        SONARQUBE_TOKEN = credentials('Sonar_Qube_Id') // Fetching the SonarQube token from Jenkins credentials store
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'git@github.com:muhammadwaqas964/Sonar.git', credentialsId: 'github-ssh-credentials'
            }
        }

        stage('Verify Maven') {
            steps {
                // Verify that Maven is correctly installed by running the `mvn -v` command
                sh 'mvn -v'
            }
        }

        stage('Build') {
            steps {
                // Clean and build the project using Maven
                sh 'mvn clean install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Perform SonarQube analysis with the configured token and project key
                withSonarQubeEnv('SonarQube') {
                    sh '''
                        mvn sonar:sonar \
                        -Dsonar.projectKey=my-simple-app \
                        -Dsonar.host.url=http://38.45.71.12:9000 \
                        -Dsonar.login=$SONARQUBE_TOKEN
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    // Wait for SonarQube Quality Gate to pass
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Quality gate failed: ${qg.status}"
                    }
                }
            }
        }

        stage('Test') {
            steps {
                // Run tests using Maven
                sh 'mvn test'
            }
        }
    }

    post {
        // After pipeline completion, whether success or failure, clean up or notify
        always {
            echo 'Pipeline execution completed'
        }
        success {
            echo 'Pipeline completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
