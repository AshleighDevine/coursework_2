pipeline {
    agent any
        stages {
            stage('Sonarqube') {
                environment {
                    scannerHome = tool 'SonarQube'
                }
                steps {
                    withSonarQubeEnv('sonarqube') {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                    timeout(time: 10, unit: 'MINUTES') {
                    					sleep(10)
                    					waitForQualityGate abortPipeline: true
                    				}
                }
            }
    }
}

node {
    def app

    stage('Build image') {
        app = docker.build("ashleighdevine/coursework2")
    }

    stage('Push image') {
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}