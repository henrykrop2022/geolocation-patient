pipeline {
    agent any
    tools {
        maven 'M2_HOME'
    }
    environment {
        BRANCH_NAME = 'main'
        PROJECT_URL = 'https://github.com/henrykrop2022/geolocation-patient.git'
        SONAQUBE_CRED = 'sonarID'
        SONAQUBE_INSTALLATION = 'Sonarqube'
        APP_NAME = 'geolocation-patient'
        SCANNER_HOME = tool 'sonar'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git branch: "$BRANCH_NAME", url: "${PROJECT_URL}"
            }
        }
        stage('Unit Test') {
            steps {
                sh 'mvn clean'
                sh 'mvn compile'
                sh 'mvn test'

            }
        }
    //     stage('SonarQube Scan') {
    //         steps {
    //              withSonarQubeEnv(credentialsId: "${SONAQUBE_CRED}", \
    //             installationName: "${SONAQUBE_INSTALLATION}" ) {
    //           sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=${APP_NAME} -Dsonar.projectKey=${APP_NAME} \
    //                -Dsonar.java.binaries=. '''
    //         }
    //     }
    // }
        stage('Trvy Scan') {
            steps {
               sh "trivy fs --format table -o maven_dependency.html ."
            }
        }
        stage('code Packaging') {
            steps {
                  sh 'mvn package'
            }
        }
    }
}