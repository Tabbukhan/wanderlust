pipeline {
    agent any
    environment{
        SONAR_HOME= tool "sonar"
    }
    stages{
        stage('Clone Code'){
            steps{
                git url: "https://github.com/krishnaacharyaa/wanderlust.git", branch: "devops"
            }
        }
        stage('Quality Scan-Sonar'){
            steps{
                withSonarQubeEnv("sonar"){
                sh "$SONAR_HOME/bin/sonar-scanner -Dsonar.projectName=wanderlust -Dsonar.projectKey=wanderlust"
                }
            }
        }
        stage('OWASP Dependency Check'){
            steps{
               dependencyCheck additionalArguments: '--scan ./', odcInstallation: 'dc'
               dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        
        /*stage('Sonar Quality GateScan'){
            steps{
                timeout(time: 2, unit: 'MINUTES')
                waitForQualityGate abortPipeline: false
            }
        }*/
        stage('Trivy File System Scan'){
            steps{
                sh 'trivy fs --format table -o trivy-fs-report.html .'
            }
        }
        /*stage('Deploy using Docker Compose'){
            steps{
                sh 'docker-compose up -d'
            }
        }*/
    }
    
}
