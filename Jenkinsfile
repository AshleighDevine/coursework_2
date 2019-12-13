node{
    def scannerHome = tool 'SonarQubeScanner';
    def app
    def remote = [:]
    remote.name = 'node-production'
    remote.host = '40.114.44.182'
    remote.user = 'master'
    remote.password = 'Password123!'
    remote.allowAnyHosts = true

  stage('Clone repository') {
        checkout scm
    }

  stage('Build image') {
        app = docker.build("ashleighdevine/coursework2")
    }

    stage('Test image') {
        withSonarQubeEnv('SonarQubeScanner') { 
            sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectName=coursework_2 -Dsonar.projectKey=coursework_2:app -Dsonar.sources=."
        }
    }

    stage('Push image') {
            docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
            app.push("${env.BUILD_NUMBER}")
            app.push("latest")
        }
    }
}