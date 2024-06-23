pipeline {
    agent any
    tools {
        maven 'maven3'
    }
    stages{
        
        stage('checkout'){
            steps{
                git url: 'https://github.com/vcjain/jenkins-sonar-demo.git', branch: 'main'
            }    
        }
        stage('Build'){
            steps{
                echo 'Building Maven project'
                sh 'mvn clean compile'
            }
        }
        stage('Test'){
            steps{
                echo 'Building Maven project'
                sh 'mvn test'
                junit '**/target/surefire-reports/*.xml'
                jacoco classPattern: '**/target/classes', exclusionPattern: '**/*Test*.class', execPattern: '**/target/jacoco.exec', inclusionPattern: '**/*.class', sourceExclusionPattern: 'generated/**/*.java', sourceInclusionPattern: '**/*.java'
            }
        }
        stage('SonarQube Analysis'){
            steps{
                echo 'Scanning Maven project'
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    withSonarQubeEnv(installationName: 'sonarcloud', credentialsId: 'sonar-token') { 
                        sh 'mvn sonar:sonar -Dsonar.projectKey=smriti-sonar_sonardemo -Dsonar.organization=smriti-sonar -Dsonar.host.url=https://sonarcloud.io'
                        sh 'sleep 50'
                        
                    }
                }    
            }
        }
    }
        

}
