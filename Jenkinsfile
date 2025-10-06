// pipeline {
//     agent any
//     environment {
//         APP_NAME = 'mywebapp'
//     }
//     stages {
//         stage('Checkout') {
//             steps {
//                 git branch: 'main', url: 'https://github.com/ArpitaKadtane/mywebapp.git'
//             }
//         }
//         stage('Build (Maven)') {
//             steps {
//                 bat 'mvn -B clean package'
//             }
//         }
//         stage('Deploy to Tomcat') {
//             steps {
//                 bat 'docker cp target\\mywebapp.war tomcat:/usr/local/tomcat/webapps/'
//                 bat 'docker restart tomcat'
//             }
//         }
//     }
//     post {
//         success {
//             echo "Deployment finished - check http://localhost:9090/${APP_NAME}/"
//         }
//         failure {
//             echo "Build or deploy failed. See console output."
//         }
//     }
// }


pipeline {
    agent any
    environment {
        APP_NAME = 'mywebapp'
        DOCKER_IMAGE = 'mytomcat:9.0'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/ArpitaKadtane/mywebapp.git'
            }
        }
        stage('Build (Maven)') {
            steps {
                bat 'mvn -B clean package'
            }
        }
        stage('Build Docker Image') {
            steps {
                // Build the Tomcat Docker image from your tomcat-docker folder
                bat 'docker build -t %DOCKER_IMAGE% tomcat-docker'
            }
        }
        stage('Deploy to Tomcat') {
            steps {
                // Remove old container if exists
                bat 'docker rm -f tomcat || exit 0'
                // Run new container with WAR deployment
                bat 'docker run -d --name tomcat -p 9090:8080 %DOCKER_IMAGE%'
                bat 'docker cp target\\mywebapp.war tomcat:/usr/local/tomcat/webapps/'
                bat 'docker restart tomcat'
            }
        }
    }
    post {
        success {
            echo "Deployment finished - check http://localhost:9090/${APP_NAME}/"
        }
        failure {
            echo "Build or deploy failed. See console output."
        }
    }
}
