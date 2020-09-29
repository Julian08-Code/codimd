pipeline {

    environment {
        imagename = 'harbor.alles.team/alles/codimd:latest'
        registryCredential = 'registry-creds'
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
                    sh 'docker build --build-arg RUNTIME=hackmdio/runtime:node-10-d27854ef -t $imagename  -f deployments/Dockerfile .'
                }
            }
        }
        
        stage('Deploy Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://harbor.alles.team', registryCredential) {
                        sh 'docker push $imagename'
                    }
                }
            }
        }

        stage('Deploy to nix1') {
            when {
                branch 'master'
            }
            steps {
                script {
                    sshagent (credentials: ['nix1-deploy']) {
                        sh 'ssh -o StrictHostKeyChecking=no -l deploy nix1.allesctf.net sudo systemctl restart codimd'
                    }
                }
            }
        }
        
    }
}