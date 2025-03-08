pipeline {
    agent any
    tools {
        maven 'M2_HOME'
    }
    environment {
        BRANCH_NAME = 'main'
        PROJECT_URL = 'https://github.com/henrykrop2022/geolocation-patient.git'
        
        SONAQUBE_INSTALLATION = 'sonar scanner'
        APP_NAME = 'geolocation-patient'
        // SCANNER_HOME = tool 'SonarQube'
        JFROG_CRED = 'jfrogID'
        ARTIFACTPATH = 'target/*.jar'
        ARTIFACTORY_URL = 'http://3.89.114.43:8082/artifactory'
        
    }
    stages {
        stage('Git Checkout') {
            steps {
                git branch: "$BRANCH_NAME", url: "$PROJECT_URL"
            }
        }
        stage('Unit Test') {
            steps {
                sh 'mvn clean'
                sh 'mvn compile'
                sh 'mvn test'
            }
        }
        stage('SonarQube Scan') {
            environment {
                SONAQUBE_CRED = 'sonarqube-ID'
                SONAR_URL = 'http://3.89.114.43:9000'
            }
                steps {
                // withSonarQubeEnv(credentialsId: "$SONAQUBE_CRED", installationName: "$SONAQUBE_INSTALLATION") {
                        sh ''' $SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.projectKey=$APP_NAME \
                        -Dsonar.host.url=$SONAR_URL \
                        -Dsonar.login=$SONAQUBE_CRED \
                        -Dsonar.projectVersion=. '''
                }
            }
        
        stage('Trivy Scan') {
            steps {
                sh "trivy fs --format table -o maven_dependency.html ."
            }
        }
        stage('Code Packaging') {
            steps {
                sh 'mvn package'
            }
        }
        // stage('Upload Jar to Jfrog') {
        //     steps {
        //         withCredentials([usernamePassword(credentialsId: "$JFROG_CRED", 
        //          usernameVariable: 'ARTIFACTORY_USER', passwordVariable: 'ARTIFACTORY_PASSWORD')]) {
        //             script {
        //                 sh """
        //                     curl -u $ARTIFACTORY_USER:$ARTIFACTORY_PASSWORD \
        //                          -T $ARTIFACTPATH \
        //                          $ARTIFACTORY_URL/repository/my-repo/release_${BUILD_ID}.jar
        //                 """
        //             }
        //         }
        //     }
        // }
    }
}
