pipeline {
    agent any
    environment {
        SONAR_HOST_URL = 'http://38.45.71.12:9000'
    }
    stages {
        stage('Build and Analyze') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'sonar-admin-credentials', passwordVariable: 'SONAR_PASSWORD', usernameVariable: 'SONAR_USER')]) {          
                    sh '''
                        sonar-scanner \  
                            -Dsonar.host.url=${SONAR_HOST_URL} \
                            -Dsonar.login=${SONAR_USER} \
                            -Dsonar.password=${SONAR_PASSWORD}  
                    '''
                }
            }
        }
    }
}
