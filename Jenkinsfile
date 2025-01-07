pipeline {
    agent any

//     tools {
//         maven 'maven3.9.9'
//     }
    environment {
        // Define environment variables
//         JAVA_HOME = '/opt/java/openjdk'  // Adjust if necessary
        JAVA_HOME = '/opt/jdk'  // Adjust if necessary
        REMOTE_SERVER = 'root@5.104.80.4'  // SSH user and VPS IP address
        DEPLOY_DIR = '/opt/jar'  // Directory on your VPS to deploy to
        JAR_NAME = 'demo-SpringBoot-SwaggerV3'  // Directory on your VPS to deploy to
        CURRENT_TIME = "${new Date().format('yyyy-MM-dd HH:mm:ss')}"
        CURRENT_TIMVvVV = "${new Date().format('yyyy-MM-dd-HH-mm-ss')}"
        ENVENV='TEST'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Clone the Git repository
                git 'https://github.com/dsl1888888/demo-SpringBoot-SwaggerV3.git'
            }
        }

        stage('Build Project') {
            steps {
                script {
                    // Run Maven to build the Spring Boot project
                    sh '''#!/bin/bash
                        mvn clean install -DskipTests
                    '''
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Optionally, run tests using Maven
                    sh 'mvn test'
                }
            }
        }

        stage('Package') {
            steps {
                script {
                    // Package the application (Spring Boot app as .jar or .war)
                    sh 'mvn package'
                }
            }
        }

        stage('Deploy to VPS') {
            steps {
                script {
                    // Copy the packaged application to the VPS using SCP
                    sh '''
                        #!/bin/bash
                        scp -P 22 target/${JAR_NAME}.jar ${REMOTE_SERVER}:${DEPLOY_DIR}/${JAR_NAME}-${ENVENV}-${CURRENT_TIMVvVV}-${BUILD_NUMBER}.jar
                    '''
                }
            }
        }

        stage('restartJava') {
            steps {
                script {
                     sh '''
                            ssh -p 22 root@5.104.80.4 << EOF || true
                            java -version
                            pkill -f 'demo-SpringBoot-SwaggerV3-${ENVENV}'
                            nohup java -jar ${DEPLOY_DIR}/${JAR_NAME}-${ENVENV}-${CURRENT_TIMVvVV}-${BUILD_NUMBER}.jar --server.port=17002 > /dev/null 2>&1 &
                            sleep 10
                            ps aux | grep demo-SpringBoot-SwaggerV3.jar
                        EOF
                    '''
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    // Optionally, verify if the app is running by checking the health endpoint (or any endpoint)
                    echo "Verifying deployment..."
                    sh '''
                        curl -f http://5.104.80.4:17002/a2 || exit 1
                    '''
                }
            }
        }





    }

    post {
        success {
            echo 'Deployment Successful!'
        }
        failure {
            echo 'Build or Deployment Failed.'
        }
    }
}
