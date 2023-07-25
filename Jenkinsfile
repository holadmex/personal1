pipeline {
    agent any
    tools {
        jdk 'JDK'
        maven 'Maven'
    }
    stages {
        stage ('PULL THE APPLICATION FROM GITHUB') {
            steps {
                git branch: 'ci-jenkins', credentialsId: 'git', url: 'https://github.com/holadmex/personal1.git'
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
                sh 'mvn verify install -DskipUnitTest'
            }
        }
        stage ('CHECKSTYLE ANALYSIS') {
            steps {
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage ('SonarQube Analysis') {
            steps{
                script{
                    withSonarQubeEnv(credentialsId: 'sonar') {
                     sh 'mvn sonar:sonar'
                    }
                }
            }
        }
        stage ('Quality Gate') {
            steps{
                script{
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar'
                }
            }
        }
        stage ('UPLOAD ARTIFACT TO NEXUS') {
            steps {
                script {
                    nexusArtifactUploader artifacts: 
                    [
                        [
                            artifactId: 'vprofile', 
                            classifier: '', file: 'target/vprofile-v2.war', 
                            type: 'war'
                            ]

                    ], 
                    credentialsId: 'nexuslogin', 
                    groupId: 'com.visualpathit', 
                    nexusUrl: '18.212.187.137:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'hey-there', 
                    version: 'v2'
                }
                }    
            }
        }
    }
  