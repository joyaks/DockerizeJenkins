pipeline {
    environment {
        registry = "jyotsnaakula/d0ckerize_jenkins"
        registryCredential = 'docker-hub'
        dockerImage=''
    }
    agent any
    tools {nodejs "NodeJS" }
    stages {
        stage('Cloning Git') {
            steps{
                # testing
                git 'https://github.com/joyaks/DockerizeJenkins.git'
            }
        }
        stage('Build') {
            steps{
                sh 'npm install'
                sh 'npm run bowerInstall'
            }    
        }
        stage('Test') {
            steps {
                sh 'npm test'
            }
        }
        stage('Building image') {
            steps{
                script {
                    dockerImage=docker.build registry + ":$BUILD_NUMBER"
                }
            }
        }
        stage('Deploy image') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
	    stage('Deploy container') {
            steps{
                script {
                    dname= registry + ":$BUILD_NUMBER"
                    sh "docker rmi -f ${dname}"
                    dockerImage.run("--name=test")
                }
            }
        }
    }
}
