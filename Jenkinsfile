pipeline {
    agent any

    environment {
        SONARQUBE_TOKEN = credentials('Sonar_Qube_Id') // Fetching the SonarQube token from Jenkins credentials store
    }

    stages {
        stage('Checkout') {
            steps {
                // Using SSH URL for the GitHub repository and specifying credentials
                git branch: 'main', url: 'git@github.com:muhammadwaqas964/Sonar.git', credentialsId: 'github-ssh-credentials'
            }
        }

        stage('Verify Maven') {
            steps {
                sh 'mvn -v'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
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
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Quality gate failed: ${qg.status}"
                    }
                }
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
    }

    post {
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
