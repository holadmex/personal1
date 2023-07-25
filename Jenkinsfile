pipeline {
    agent any
    tools {
        jdk 'JDK'
        maven 'Maven'
    }
    enviroment {
        USERNAME = 'admin'
        nexuspassword = 'Janet125'
        nexuslogin = 'admin'
        nexusip = '18.212.187.137'
        nexusport = '8081'
        centralrepo = 'hey-there'
        releaserepo = 'hey-there1'
        nexusgroup = 'hey-there-group'
        SNAPrepo = 'hey-there2'

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
        post{
            success{
                echo 'now archiving'
                archiveArtifacts artifacts: '**/*.war', followSymlinks: false
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
            steps{
                nexusArtifactUploader {
                    nexusVersion: 'nexus3'
                    protocol: 'http'
                    nexusurl: "${nexusip}:${nexusport}",
                    groupid: 'QA',
                    version: "${env_BUILD}-${env_TIMESTAMP}",
                    repository: "${release repo}",
                    credentialsId: "${nexuslogin}",
                    artifacts: [
                        [artifactId: 'hey-thereapp',
                        classifier: '',
                        file: 'target/vprofile-v2.war'
                        type: 'war']
                    ]
                }    
            }
        }
    }
}    