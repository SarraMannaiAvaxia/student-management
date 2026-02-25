pipeline {
    agent any

    tools {
        jdk 'JDK21'
        maven 'maven3'      // use the exact name you have in Global Tool Configuration
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Code already checked out by Jenkins SCM'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install -DskipTests'   // or without -DskipTests if you added H2
            }
        }
     stage('MAVEN Build') {
            steps {
                // Compile + package (no tests for now to avoid DB issues)
                // Remove -DskipTests if you added H2 for tests
                sh 'mvn clean compile -DskipTests'
                // Alternative full build: sh 'mvn clean install -DskipTests'
            }
        }

        stage('SONARQUBE') {
            environment {
                SONAR_HOST_URL   = 'http://192.168.33.10:9000/'
                SONAR_AUTH_TOKEN = credentials('sonarqube')   // Must match your Credential ID in Jenkins
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
    }
}
        stage('Done') {
            steps {
                echo '=================================='
                echo '   Student Management BUILD OK   '
                echo '=================================='
            }
        }
    }
}
