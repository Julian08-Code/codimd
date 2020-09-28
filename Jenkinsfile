pipeline {

    environment {
        imagename = 'alles/codimd'
        registryCredential = 'registry-creds'
        dockerImage = ''
    }

    agent {
        label 'linux'
    }

    options {
        timeout(time: 10, unit: 'MINUTES')
    }

    triggers {
        githubPush()
    }

    stages {
        stage('Building image') {
            steps {
                script {
                    sh 'docker build -t $imagename  -f deployments/Dockerfile .'
                }
            }
        }
        /*
        stage('Deploy Image') {
            steps {
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push("$BUILD_NUMBER")
                        dockerImage.push('latest')
                    }
                }
            }
        }
        */
    }
}