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

        stage('Done') {
            steps {
                echo '=================================='
                echo '   Student Management BUILD OK   '
                echo '=================================='
            }
        }
    }
}