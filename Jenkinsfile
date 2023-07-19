pipeline{
    agent any
    tools {
        jdk 'JDK'
        maven 'Maven'
    }
    environment {
        SONAR_SCANNER = 'SonarQube Scanner'
        SONAR_SERVER = 'SonarQube Server'
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
                sh 'mvn test'
            }
        }
        stage ('UNIT TEST') {
            steps {
                sh 'mvn test'
            }
        }
        stage ('INTEGRATION TEST') {
            steps {
                sh 'mvn verify -DskipUnitTest'
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
        stage ('Quality Gate') {
            steps{
                script {
                    timeout (timeout: 1, unit: 'MINUTES') {
                        waitForQualityGate abortPipeline: false, credentialsId: 'sonar'
                }
                    }
                }    
            }
        }
    }
