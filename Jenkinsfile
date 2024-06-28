pipeline {
    agent any

    environment {
        SONARQUBE_SERVER = 'SonarQube'       // SonarQube server name configured in Jenkins
        SONARQUBE_SCANNER = 'SonarScanner'   // SonarQube Scanner tool name configured in Jenkins
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the repository
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/narharigtm/Devops-Html-CSS-Project.git']])
            }
        }
        
        stage('SonarQube Analysis') {
            steps {
                script {
                    // Define SonarQube analysis parameters
                    def sonarProps = '''
                        sonar.projectKey=my-web-project
                        sonar.projectName=My Web Project
                        sonar.projectVersion=1.0
                        sonar.sources=src
                        sonar.inclusions=**/*.html,**/*.css
                        sonar.language=html
                        sonar.sourceEncoding=UTF-8
                    '''
                    writeFile file: 'sonar-project.properties', text: sonarProps

                    // Run SonarQube analysis
                    withSonarQubeEnv('SonarQube') {
                        def scannerHome = tool 'SonarScanner'
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        
        stage('Quality Gate') {
            steps {
                script {
                    // Wait for SonarQube analysis to complete
                    timeout(time: 1, unit: 'HOURS') {
                        def qualityGate = waitForQualityGate()
                        if (qualityGate.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qualityGate.status}"
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            // Archive the SonarQube analysis report
            archiveArtifacts artifacts: '**/target/*.html', allowEmptyArchive: true
        }
    }
}
