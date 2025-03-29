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
                    bat 'echo Deploying to staging environment'
                    deployToEnv('staging', 7002)
                }
            }
        }
        stage('tests-on-staging') {
            steps {
                script {
                    bat 'echo Testing staging environment'
                    runTests('staging')
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

        set HOMEPATH=C:\\Users\\%USERNAME%\\AppData\\Roaming
        set HOME=%HOMEPATH%

        pm2 delete greetings-app-$env || EXIT /B 0
        if ERRORLEVEL 1 (
            echo "No running process found for greetings-app-$env, skipping deletion."
        )

        timeout /t 3

        echo "te vel strada."

        pm2 start app.py --name greetings-app-$env -- --port $port
    """
}

def runTests(env) {
    bat """
        echo "Tiek izpildīti testi $env videi..."

        where npm

        if not exist "course-js-api-framework" (
            git clone https://github.com/mtararujs/course-js-api-framework
        ) else (
            echo "Repozitorijs jau eksistē, izlaižam klonēšanu."
        )
        
        cd course-js-api-framework
        
        call npm install > npm_install_output.log 2>&1
        type npm_install_output.log

        echo "Running tests..."
        echo "Available npm scripts:"
        npm run
       
        timeout /t 3
        echo "Running the greetings test script..."
        call npm run greetings greetings_$env > greetings_output.log 2>&1
        type greetings_output.log

        if %ERRORLEVEL% NEQ 0 (
            echo "Tests failed for $env!" 
            exit /b 1
        )
    """
}