pipeline {
    agent any
    tools {
        jdk 'JDK'
        maven 'Maven'
    }
    stages {
        stage ('PULL THE APPLICATION FROM GITHUB') {
            steps {
                git branch: 'ci-jenkins', url: 'https://github.com/afolagbe/personal.git'
            }
        }
        stage ('BUILD THE APPLICATION') {
            steps {
                sh 'mvn install -DeskipTest'
            }
        }
        stage ('TEST') {
            steps {
                sh 'mvn -s setting.xml test'
            }
        }
        stage ('UNIT TEST') {
            steps {
                sh 'mvn -s setting.xml test'
            }
        }
        stage ('INTEGRATION TEST') {
            steps {
                sh 'mvn -s setting.xml verify -DskipUnitTest'
            }
        }
        stage ('CHECKSTYLE ANALYSIS') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage ('SonarQube analysis') {
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar') {
                     sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
        stage ("Quality Gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar'
                }
            }
    
        }
    }    
}  
