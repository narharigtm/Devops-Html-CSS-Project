pipeline {
    agent any

    stages {
        stage('Clone') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/narharigtm/PhpMysql_project.git']])
            }
        }
        stage('build & run'){
            steps{
                sh 'docker-compose up -d'
                sh 'docker ps'
            }
        }
    }
    post {
        success {
            echo 'Pipeline succeeded!'
            sh 'docker-compose down'
        }
        failure {
            echo 'Pipeline failed!'
            sh 'docker-compose down'
        }
    }
}
