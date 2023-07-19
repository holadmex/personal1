pipeline {
    agent any
    tools {
        jdk 'JDK'
        maven 'Maven'
    }
    stages {
        stage ('BUILD') {
            steps {
                sh 'mvn -s setting.xml install -DskiptTest'
            }
            post {
                success {
                    echo 'Now archiving'
                    archiveArtifacts artifacts: 'target/*war*', followSymlinks: false
                }
            }
        }
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
