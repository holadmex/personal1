pipeline {
    agent any
    tools{
        jdk 'JDK'
        maven 'Maven'
    }
    stages{
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