pipeline {
    agent any
    tools {
        maven 'M2_HOME'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/henrykrop2022/geolocation-patient.git'
            }
        }
    }
}