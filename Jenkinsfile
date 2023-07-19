pipeline {
    agent any
    tools{
        jdk 'JDK'
        maven 'Maven'
    }
    stages {
        stage ('FETCH CODE FROM GITHUB') {
            steps{
                git branch: 'ci-jenkins', url: 'https://github.com/holadmex/personal1.git'
            }
        }
        stage('BUILD'){
            steps{
                sh 'mvn -s settings.xml install -DskipTests'
            }
            post{
                success{
                    echo 'now archiving'
                    archiveArtifacts artifacts: 'target/*war*', followSymlinks: false
                }
            }
        }
        stage('TEST'){
            steps{
                sh 'mvn -s settings.xml test'
            }
        }
    }
}    