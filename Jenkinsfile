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
        stage('deploy-to-dev') {
            steps {
                script {
                    sh 'echo Deploying to development environment'
                    deployToEnv('dev', 7001)
                }
            }
        }
        stage('deploy-to-preprod') {
            steps {
                script {
                    sh 'echo Deploying to preprod environment'
                    deployToEnv('preprod', 7003)
                }
            }
        }
        stage('deploy-to-prod') {
            steps {
                script {
                    sh 'echo Deploying to production environment'
                    deployToEnv('prod', 7004)
                }
            }
        }

    }
}

def installPipDeps() {
    sh '''
        echo "Klonē 'python-greetings' repozitoriju..."

        if [ ! -d "python-greetings" ]; then
            git clone https://github.com/mtararujs/python-greetings
        else
            echo "Repozitorijs jau eksistē"
        fi

        cd python-greetings
        ls
        python3 -m venv ./venv
        bash -c "source ./venv/bin/activate && pip install -r requirements.txt"
    '''
}

def deployToEnv(env, port) {
    sh """
        echo "Izvieto $env vidē..."

        if [ ! -d "python-greetings" ]; then
            git clone https://github.com/mtararujs/python-greetings
        else
            echo "Repozitorijs jau eksistē"
        fi  

        cd python-greetings
        pm2 delete greetings-app-$env || true
        pm2 start app.py --name greetings-app-$env -- --port $port
    """
}