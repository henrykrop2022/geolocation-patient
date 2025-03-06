pipeline {
    agent any
    tools {
        maven 'M2_HOME'
    }
    environment {
        BRANCH_NAME = 'main'
        PROJECT_URL = 'https://github.com/henrykrop2022/geolocation-patient.git'
        SONAQUBE_CRED = 'sonar-tokenID'
        SONAQUBE_INSTALLATION = 'Sonarqube'
        SONAR_URL = 'http://50.16.177.186:9000'
        APP_NAME = 'geolocation-patient'
        SCANNER_HOME = tool 'Sonar'
        JFROG_CRED = 'jfrogID'
        ARTIFACTPATH = 'target/*.jar'
        ARTIFACTORY_URL = 'http://54.197.33.41:8082/artifactory'
        BUILD_ID = 'env.BUILD_ID'
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
        stage('SonarQube Scan') {
            steps {
                 withSonarQubeEnv(credentialsId: "${SONAQUBE_CRED}", \
                installationName: "${SONAQUBE_INSTALLATION}" ) {
              sh ''' $SCANNER_HOME/bin/sonar-scanner  -Dsonar.projectKey=$"{APP_NAME}" \-Dsonar.host.url=$"{SONAR_URL}" \ -Dsonar.login=$"{SONAQUBE_CRED}" -Dsonar.projectVersion=$"{BUILD_ID}" '''
            }
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
        //  stage('Upload Jar to Jfrog'){
        //     steps{
        //         withCredentials([usernamePassword(credentialsId: "${JFROG_CRED}", \
        //          usernameVariable: 'ARTIFACTORY_USER', passwordVariable: 'ARTIFACTORY_PASSWORD')]) {
        //             script {
        //                 // Define the artifact path and target location
        //                 //def artifactPath = 'target/*.jar'
        //                 //def targetPath = "release_${BUILD_ID}.jar"

        //                 // Upload the artifact using curl
        //                 sh """
        //                     curl -u ${ARTIFACTORY_USER}:${ARTIFACTORY_PASSWORD} \
        //                          -T ${ARTIFACTPATH} \
        //                          ${ARTIFACTORY_URL}/${REPO}/${ARTIFACTTARGETPATH}
        //                 """
        //     }
//         }
//     }

// }
    }
}