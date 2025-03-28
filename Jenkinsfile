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
        stage('tests-on-dev') {
            steps {
                script {
                    sh 'echo Testing development environment'
                    runTests('dev')
                }
            }
        }
        stage('deploy-to-staging') {
            steps {
                script {
                    sh 'echo Deploying to staging environment'
                    deployToEnv('staging', 7002)
                }
            }
        }
        stage('tests-on-staging') {
            steps {
                script {
                    sh 'echo Testing staging environment'
                    runTests('staging')
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
        stage('tests-on-preprod') {
            steps {
                script {
                    sh 'echo Testing preprod environment'
                    runTests('preprod')
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
        stage('tests-on-prod') {
            steps {
                script {
                    sh 'echo Testing production environment'
                    runTests('prod')
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

def runTests(env) {
    sh """
        echo "Tiek izpildīti testi $env videi..."

        if [ ! -d "course-js-api-framework" ]; then
            git clone https://github.com/mtararujs/course-js-api-framework
        else
            echo "Repository already exists, skipping clone."
        fi
        
        cd course-js-api-framework
        npm install
        npm run greetings greetings_$env
    """
}
