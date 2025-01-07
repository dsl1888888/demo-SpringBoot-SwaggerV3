pipeline {
    agent any
//     tools {
//         //git 'Git'  // This refers to the Git installation you configured in Jenkins
// //         mvn 'maven3.9.9'
//     }
    environment {
        // The branch name can be accessed as an environment variable.
        BRANCH_NAME = "${env.GIT_BRANCH}"
    }



    stages {
        // Stage to check out the source code from the Git repository
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        // Stage to build the Spring Boot application using Maven
        stage('Build') {
            agent { docker 'maven:3-alpine' } 
            steps {
                script {
                    echo "Building Spring Boot application from branch: ${BRANCH_NAME}"

                    // Run Maven clean and install commands to build the project
                    sh 'mvn clean install -DskipTests'
                }
            }
        }

        // Stage to run tests (you can configure it to run only on specific branches or environments)
//         stage('Test') {
//             steps {
//                 script {
//                     echo "Running tests for branch: ${BRANCH_NAME}"
//
//                     // Run Maven test command
//                     sh 'mvn test'
//                 }
//             }
//         }

        // Stage to deploy the Spring Boot application (this can be to a test environment)
//         stage('Deploy') {
//             steps {
//                 script {
//                     echo "Deploying branch: ${BRANCH_NAME}"
//
//                     // Run Maven to package and deploy to a test environment
//                     sh 'mvn spring-boot:run -Dspring-boot.run.profiles=test'
//                 }
//             }
//         }
    }

    // Post actions to handle success or failure notifications
    post {
        success {
            echo "Pipeline for branch ${BRANCH_NAME} completed successfully."
        }
        failure {
            echo "Pipeline for branch ${BRANCH_NAME} failed."
        }
    }
}
