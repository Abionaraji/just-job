def COLOR_MAP =[
    'SUCCESS' : 'good'
    'FAILURE' : 'danger'
]
pipeline{
    agent any
    tools {
        jdk 'JDK'
        maven 'maven'
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
                sh ' mvn test'
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
    }
    post {
        aways{
            echo 'slack notifications'
            slankSend channel: '#ci-project'
            color: COLOR_MAP[currentBuild.currentResult],
            message: "*${currentBuild.currentResult}:*job ${evn.JOB_NAME} build ${evn.Build_name} time ${evn.BUILD_TIMESTAMP} \n More info at: ${BUILD URL}"
        }
    }
}