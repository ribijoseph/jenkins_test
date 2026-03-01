pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
       
        triggers {
            githubPush()
        }

        stage ('Build') {
            steps {
                sh 'docker build -t python-app:${BRANCH_NAME} .'
            }
        }
        
        stage('Test') {
            steps {
                sh 'docker run --rm python-app:${BRANCH_NAME} pytest test_app.py'
            }
        }

        stage('Deploy') {
            steps {
                script {
                    
                    if (env.BRANCH_NAME=="feature") {
                        sh '''
                        docker-compose down
                        docker-compose up -d dev
                        '''
                    }

                    else if (env.BRANCH_NAME == "develop") {
                        sh '''
                        docker-compose down
                        docker-compose up -d qa
                        '''
                    }

                    else if (env.BRANCH_NAME == "main") {
                        sh '''
                        docker-compose down
                        docker-compose up -d uat
                        '''
                    }
                }
            }
        }
    }
}