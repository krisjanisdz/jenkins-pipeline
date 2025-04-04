pipeline {
    agent any
    stages {
        stage('install-pip-deps') {
            steps {
                script {
                    bat 'echo Installing dependencies...'
                    installPipDeps()
                }
            }
        }
        stage('deploy-to-dev') {
            steps {
                script {
                    bat 'echo Deploying to development environment'
                    deployToEnv('dev', 7001)
                }
            }
        }
        stage('tests-on-dev') {
            steps {
                script {
                    bat 'echo Testing development environment'
                    runTests('dev')
                }
            }
        }
        stage('deploy-to-staging') {
            steps {
                script {
                    bat 'echo Deploying to development environment'
                    deployToEnv('stg', 7002)
                }
            }
        }
        stage('tests-on-staging') {
            steps {
                script {
                    bat 'echo Testing development environment'
                    runTests('stg')
                }
            }
        }
        stage('deploy-to-preprod') {
            steps {
                script {
                    bat 'echo Deploying to preprod environment'
                    deployToEnv('preprod', 7003)
                }
            }
        }
        stage('tests-on-preprod') {
            steps {
                script {
                    bat 'echo Testing preprod environment'
                    runTests('preprod')
                }
            }
        }
        stage('deploy-to-prod') {
            steps {
                script {
                    bat 'echo Deploying to production environment'
                    deployToEnv('prod', 7004)
                }
            }
        }
        stage('tests-on-prod') {
            steps {
                script {
                    bat 'echo Testing production environment'
                    runTests('prod')
                }
            }
        }
    }
}

def installPipDeps() {
    bat '''
        echo "Klonē 'python-greetings' repozitoriju..."

        if not exist "python-greetings" (
            git clone https://github.com/mtararujs/python-greetings
        ) else (
            echo "Repozitorijs jau eksistē"
        )

        cd python-greetings
        pip install -r requirements.txt
    '''
}

def deployToEnv(env, port) {
    bat """
        echo "Izvieto $env vidē..."

        if not exist "python-greetings" (
            git clone https://github.com/mtararujs/python-greetings
        ) else (
            echo "Repozitorijs jau eksistē"
        )  

        cd python-greetings

        pm2 list | findstr "greetings-app-$env" > nul
        set ERRORLEVEL_1=%ERRORLEVEL%

        rem handle the error level
        if %ERRORLEVEL_1% EQU 0 (
            echo "Found a running process for greetings-app-$env. Deleting it..."
            pm2 delete greetings-app-$env
            rem Check if delete failed
            if %ERRORLEVEL% NEQ 0 (
                echo "Failed to delete process, but continuing..."
            )
        ) else (
            echo "No running process found for greetings-app-$env, skipping deletion."
        )

        pm2 start app.py --name greetings-app-$env -- --port $port
    """
}

def runTests(env) {
    bat """
        echo "Tiek izpildīti testi $env videi..."

        if not exist "course-js-api-framework" (
            git clone https://github.com/mtararujs/course-js-api-framework
        ) else (
            echo "Repozitorijs jau eksistē, izlaižam klonēšanu."
        )

        cd course-js-api-framework
        call npm install 
        echo "Running the greetings test script..."
        npm run greetings greetings_$env
    """
}