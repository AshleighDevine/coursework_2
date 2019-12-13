pipeline {
    agent any
    environment {
        CI = 'true'
    }
    stages {
       stage('SonarQube') {
    environment {
        scannerHome = tool 'SonarQubeScanner'
    }
    steps {
        withSonarQubeEnv('sonarQube') {
            sh "${scannerHome}/bin/sonar-scanner"
        }
        timeout(time: 10, unit: 'MINUTES') {
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