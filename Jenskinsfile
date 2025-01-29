pipeline {
    agent { label 'server1-irfan' }

    stages {
        stage('Pull SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/rzaykey/simple-apps.git'
            }
        }
        
        stage('Build') {
            steps {
                sh'''
                cd app
                npm install
                '''
            }
        }
        
        stage('Testing') {
            steps {
                sh'''
                cd app
                APP_PORT=4001 npm test
                APP_PORT=4001 npm run test:coverage
                '''
            }
        }
        
        stage('Code Review') {
            steps {
                sh'''
                cd app
                sonar-scanner   -Dsonar.projectKey=simple-apps   -Dsonar.sources=.   -Dsonar.host.url=http://172.23.3.12:9000   -Dsonar.login=sqp_25e99e53e2f268cbcf152da97906e1923e0d7309
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                sh'''
                docker compose up --build -d
                '''
            }
        }
        
        stage('Backup') {
            steps {
                 sh 'docker compose push' 
            }
        }
    }
}