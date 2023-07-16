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
                stage('CHECKSTYLE ANALYSIS'){
            steps{
                sh 'mvn -s settings.xml checkstyle:checkstyle'
            }
        }
                stage ('SONAR ANALYSIS') {
            environment {
                scannerHome = tool "${ SONAR_SCANNER}"
            }
            steps {
                withSonarQubeEnv("${SONAR_SERVER}") {
               sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=vprofile \
                   -Dsonar.projectName=vprofile \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
            }
        }
    }
    }
}