pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = "aromaljosephkm/test:1"
    }
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        stage('Build Docker Image') {
            steps {
                // script {
                //     app = docker.build("aromaljosephkm/test:1")
                //     app.inside {
                //         sh 'echo Hello, World!'
                //     }
                // }
                sh """
                    echo "Image Build Successfull"
                """
            }
        }
        // stage('Push Docker Image') {
        //     steps {
        //         script {
        //             docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
        //                 app.push("${env.BUILD_NUMBER}")
        //                 app.push("latest")
        //             }
        //         }
        //     }
        // }
        stage('CanaryDeploy') {
            steps {
                sh """
                    echo "Canary Deployment Successfull"
                """
            } 
        }
        stage('DeployToProduction') {
            steps {
                input 'Deploy to Production'
                sh """
                    echo "Production Deployment Successfull"
                """ 
            }
        }
    }
}
