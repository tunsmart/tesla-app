pipeline {
    agent any
    
    tools {
        maven ("Maven")
    }

    stages {
        stage('Check out code') {
            steps {
                git credentialsId: 'GithubCredentials', url: 'https://github.com/tunsmart/tesla-app'
            }
        }
        
        //build
        stage ('Build and Sonarqube report in parallel') {
            steps {
                parallel (Build: {sh "mvn clean package"}, SonarQubeReport: {sh "mvn sonar:sonar"})
                    
            }
        }
        
        stage ('Upload artifacts to nexus') {
            steps {
                 sh "mvn deploy"
            }
        }
        
        stage('deploy to UAT') {
            steps {
                 sh "echo 'deploy to UAT'  "
                deploy adapters: [tomcat9(credentialsId: '5f98e193-f988-46ab-9d4d-defb5a96d61a', path: '', url: 'http://18.118.147.83:8080/')], contextPath: null, war: 'target/*war'
            }
        }

    }
}
