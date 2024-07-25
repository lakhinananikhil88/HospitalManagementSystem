pipeline {
    agent any

    tools {
        maven 'apache-maven-3.9.8'
        jdk 'JDK17'
    }

    environment {
        GITHUB_REPO = 'https://github.com/lakhinananikhil88/HospitalManagementSystem.git'
        TOMCAT_URL = 'http://localhost:9090/manager/text'
        TOMCAT_USER = 'admin'
        TOMCAT_PASS = 'tomcat'
        GITHUB_USER = 'lakhinananikhil88'
        GITHUB_PASS = 'Aiyura@2016'
    }

    stages {
        stage('Initialize') {
            steps {
                bat '''
                    echo PATH = %PATH%
                    echo M2_HOME = %M2_HOME%
                    echo JAVA_HOME = %JAVA_HOME%
                '''
            }
        }

        stage('Clone Repository') {
            steps {
                script {
                    def gitUrl = GITHUB_REPO.replace('https://', "https://${GITHUB_USER}:${GITHUB_PASS}@")
                    git url: gitUrl, branch: 'master'
                }
            }
        }

        stage('Build HospitalManagementSystem Project') {
            steps {
                bat 'mvn clean compile package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube10V') {
                    bat "mvn sonar:sonar"
                }
            }
        }

        stage('Deploy to Tomcat') {
            steps {
                script {
                    deploy adapters: [tomcat9(path: '', url: '')], contextPath: 'HospitalManagementSystem', onFailure: false, war: '**/*.war'
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully.'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}
