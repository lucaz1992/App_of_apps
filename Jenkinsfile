def frontendImage="lzabinsk/panda-front"
def backendImage="lzabinsk/panda-back"

pipeline {
    agent {
        label 'agent'
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
    }
}
