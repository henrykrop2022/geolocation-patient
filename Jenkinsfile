pipeline {
    agent any
    tools {
        maven 'M2_HOME'
    }
    environment {
        BRANCH_NAME = 'main'
        PROJECT_URL = 'https://github.com/henrykrop2022/geolocation-patient.git'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git branch: "$BRANCH_NAME", url: "${PROJECT_URL}"
            }
        }
    }
}