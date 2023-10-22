pipeline{
    agent any
    tools {
        jdk 'JDK'
        maven 'Maven'
    }
    stages {
        stage ('FETCH THE CODE FROM GITHUB') {
            steps{
                git branch: 'ci-jenkins', credentialsId: 'gitlogin', url: 'git@github.com:holadmex/personal1.git'
            }
        }
        stage ('BUILD THE CODE') {
            steps{
                sh 'mvn install -DskipTest'
            }
            post {
                success {
                    echo 'Archiving artifacts now.'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage ('UNIT TEST') {
            steps{
                sh 'mvn test'
            }
        }
        stage ('Checkstyle Analysis') {
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage ('Sonar Analysis') {
            environment {
                scannerHome = tool 'sonar4.7'
            }
            steps{
                withSonarQubeEnv('sonar') {
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
        stage ('Quality Gate') {
            steps{
                timeout(time: 2, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }    
            }
        }
    }
    stage ('Upload Artifact') {
        steps{
            nexusArtifactUploader(
                nexusVersion: 'nexus3',
                  protocol: 'http',
                  nexusUrl: '172.31.93.30:8081',
                  groupId: 'QA',
                  version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                  repository: 'vprofile-repo',
                  credentialsId: 'nexuslogin',
                  artifacts: [
                    [artifactId: 'vproapp1',
                     classifier: '',
                     file: 'target/vprofile-v2.war',
                     type: 'war']
                  ]
            )
        }
    }
}