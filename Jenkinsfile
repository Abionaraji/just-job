pipeline{
    agent any
    tools {
        jdk 'JDK'
        maven 'Maven'
    }
    environment {
        NEXUSUSER = 'admin'
        nexuspassword = 'admin'
        releaserepo = 'vpro-release'
        nexusip = '54.160.87.41'
        nexusport = '8081'
        nexuslogin = 'nexus-jenkins'
    }
    stages {
        stage ('PULL THE APPLICATION FROM GITHUB') {
            steps {
                git branch: 'ci-jenkins', url: 'https://github.com/Abionaraji/just-job.git'
            }
        }
        stage ('BUILD THE APPLICATION') {
            steps {
                sh 'mvn clean install' 
            }
            post {
                success{
                    echo 'New achiving'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage ('TEST') {
            steps {
                sh ' mvn test'
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
        stage('UPLOAD ARTIFACT TO NEXUS'){
            steps{
                nexusArtifactUploader{
                    nexusVersion: "nexus3",
                    protocol: "${NEXUS_IP}",
                    nexusUrl:"${NEXUS_IP}:${NEXUS_PORT}",
                    groupId: "QA",
                    version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",
                    repository: "${RELEASE_REPO}",
                    credentialsld: "${NEXUS_LOGIN}",
                    artifacts[
                        [artifact:'vproapp',
                        classifier:"",
                        file:'target/vprofile-v2.war',
                        type:'war']
                    ]
                }
            }
        }
    }
}
