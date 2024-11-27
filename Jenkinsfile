def frontendImage="lzabinsk/panda-front"
def backendImage="lzabinsk/panda-back"

pipeline {
    agent {
        label 'agent'
    }
    
    environment { 
        PIP_BREAK_SYSTEM_PACKAGES = 1
    }

    tools {
        terraform 'Terraform'
    }

    parameters {
        string(name: 'backendDockerTag', defaultValue: 'latest', description: 'Backend tag')
        string(name: 'frontendDockerTag', defaultValue: 'latest', description: 'Frontend tag')
    }

    stages {
        stage('Get Code') {
            steps {
                checkout scm
            }
        }
        stage('Adjust version'){
            steps{
                script{
                    currentBuild.description = "Wersja backendu ${backendDockerTag}, Wersja: ${frontendDockerTag}"
                }
            }
        }

        stage('Remove containers') {
            steps {
                sh "docker rm -f panda-front panda-back"
            }
        }

        stage('Deploy app'){
            steps {
                script {
                    withEnv(["FRONTEND_IMAGE=$frontendImage:$frontendDockerTag",
                    "BACKEND_IMAGE=$backendImage:$backendDockerTag"]) {
                        sh "docker compose up -d"
                    }
                }
            }
        }

        stage('Selenium tests') {
            steps {
                sh "pip3 install -r test/selenium/requirements.txt"
                sh "python3 -m pytest test/selenium/frontendTest.py"
            }
        }
    }

    post {
        always {
            sh "docker-compose down"
            cleanWs()
        }
    }
}
