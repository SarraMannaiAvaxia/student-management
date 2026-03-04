pipeline {
    agent any

    tools {
        jdk 'JDK21'     // exact name from your Global Tool Configuration
        maven 'maven3'  // exact name (lowercase m)
    }

    stages {
        stage('Checkout GIT') {
            steps {
                echo 'Pulling student-management project from GitHub...'
                git branch: 'main',
                    url: 'https://github.com/SarraMannaiAvaxia/student-management.git'
            }
        }

        stage('MAVEN Build') {
            steps {
                // Compile + package, skip tests to avoid DB connection issues
                // Remove -DskipTests if you added H2 in-memory DB for tests
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('SONARQUBE Analysis') {
            environment {
                SONAR_HOST_URL   = 'http://192.168.33.10:9000/'  // your VM IP:9000
                SONAR_AUTH_TOKEN = credentials('Sonarqube')      // credential ID must match exactly
            }
            steps {
                sh '''
                    mvn sonar:sonar \
                      -Dsonar.projectKey=student-management \
                      -Dsonar.host.url=${SONAR_HOST_URL} \
                      -Dsonar.login=${SONAR_AUTH_TOKEN}
                '''
            }
        }

        stage('Done') {
            steps {
                echo '=================================='
                echo '   Student Management BUILD OK   '
                echo '   SonarQube analysis completed   '
                echo '=================================='
            }
        }
    }
}
