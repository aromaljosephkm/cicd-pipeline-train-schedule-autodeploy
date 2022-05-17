pipeline {
    agent any
    environment {
        DOCKER_IMAGE_NAME = "aromaljosephkm/test"
        DOCKERHUB_CREDENTIALS=credentials('dockerhub')
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
                script {
                    app = docker.build(DOCKER_IMAGE_NAME)
                    app.inside {
                        sh 'echo Hello, World!'
                    }
                }
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

        stage('Login') {

			steps {
				sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push $DOCKER_IMAGE_NAME'
			}
		}

        stage('CanaryDeploy') {
            steps {
                sh """
                kubectl apply -f train-schedule-kube-canary.yml
            """
            }
            
            
        }
        stage('DeployToProduction') {
            steps {
                input 'Deploy to Production'
                sh """
                    kubectl apply -f train-schedule-kube.yml
                """
                
            }
        }
    }
}
