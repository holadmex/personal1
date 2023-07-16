pipeline {
    agent any
    tools {
        jdk 'JDK'
        maven 'Maven'
    }
    stages {
        stage ('FETCH CODE FROM GITHUB') {
            steps{
                git branch: 'ci-jenkins', url: 'https://github.com/holadmex/personal1.git'
            }
        }
        stage ('BUILD THE CODE') {
            steps {
                sh 'mvn install -DskipTest'
            }
        }    
        stage ('TEST THE CODE') {
            steps {
                sh 'mvn test'
            }
        }
        stage ('INTEGRATION UNIT TEST') {
            steps {
                sh 'mvn install -DskipUnitTest'
            }
        }
    }
}    