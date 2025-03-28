pipeline {
    agent any
    stages {
        stage('install-pip-deps') {
            steps {
                script {
                    sh 'echo Installing dependencies...'
                    installPipDeps()
                }
            }
        }
    }
}

def installPipDeps() {
    sh '''
        echo "Klonē 'python-greetings' repozitoriju...."
        git clone https://github.com/mtararujs/python-greetings
        cd python-greetings
        ls
        echo "Instalē Python dependencies...."
        pip3 install -r requirements.txt
    '''
}