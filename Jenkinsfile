pipeline {
    agent any

    stages {
        stage('install-pip-deps') {
            steps {
                script{
                    build()
                }
            }
        }
        stage('deploy-to-dev') {
            steps {
                script{
                    deploy("dev", 7001)
                } 
            }
        }
        stage('tests-on-dev') {
            steps {
                script{
                    testing("greetings", "dev")
                }
            }
        }
        stage('deploy-to-staging') {
            steps {
                script{
                    deploy("staging", 7002)
                }
            }
        }
        stage('tests-on-staging') {
            steps {
                script{
                    testing("greetings", "staging")
                }
            }
        }
        stage('deploy-to-preprod') {
            steps {
                script{
                    deploy("preprod", 7003)
                }
            }
        }
        stage('tests-on-preprod') {
            steps {
                script{
                    testing("greetings", "preprod")
                }
            }
        }
        stage('deploy-to-prod') {
            steps {
                script{
                    deploy("prod", 7004)
                }
            }
        }
        stage('tests-on-prod') {
            steps {
                script{
                    testing("greetings", "prod")
                }
            }
        }
    }
}


def build(){
    echo "Installing all required depdendencies.."
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/python-greetings'
    bat "dir"
    bat "pip install -r requirements.txt"
}

def deploy(String environment, int port){
    echo "Deployment to ${environment} has started.."
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/python-greetings '
    bat "node_modules\\.bin\\pm2 delete \"greetings-app-${environment}\" & set errorlevel=0"
    bat "node_modules\\.bin\\pm2 start app.py --name \"greetings-app-${environment}\" -- --port ${port}"
}

def testing(String test_set, String environment){
    echo "Testing test set on ${environment} has started.."
    git branch: 'main', poll: false, url: 'https://github.com/mtararujs/course-js-api-framework'
    bat "npm install"
    bat "npm run ${test_set} ${test_set}_${environment}"
}