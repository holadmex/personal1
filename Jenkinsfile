pipeline {
    agent any
    tools {
        jdk 'JDK'
        maven 'Maven'
    }
    stages {
        stage ('FETCH CODE FROM GITHUB') {
            steps{
                git branch: 'ci-jenkins', credentialsId: 'gitlogin', url: 'git@github.com:holadmex/personal1.git'
            }
        }
        stage ('BUILD THE CODE') {
            steps{
                sh 'mvn install -DskipTest'
            }
        }
        post {
            success {
                echo 'Archiving artifacts now.'
                archiveArtifacts artifacts: '**/*.war'
            }
        }
        stage ('UNIT TEST') {
            steps{
                sh 'mvn test'
            }
        }
    }
}