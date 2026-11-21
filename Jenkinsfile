pipeline {

    agent any

    options {
        skipDefaultCheckout(true)
        timestamps()
        disableConcurrentBuilds()
        timeout(time: 20, unit: 'MINUTES')
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }

    tools {
        maven 'Maven'
        jdk 'Java'
        // git 'Git'
    }

    stages {

        stage('Checkout') {
            steps {
                echo '📥 Getting code...'
                checkout scm
            }
        }

        stage('Build') {
            steps {
                echo '🔨 Building...'
                sh 'mvn -B -ntp clean compile'
            }
        }

        stage('Test') {
            steps {
                echo '🧪 Testing...'
                sh 'mvn -B -ntp test -Dspring.profiles.active=h2 -Dlogging.file.name=target/application.log'
            }
        }

        stage('Package') {
            steps {
                echo '📦 Creating JAR...'
                sh 'mvn -B -ntp package -DskipTests'
            }
        }
    }

    post {
        always {
            junit testResults: 'target/surefire-reports/TEST-*.xml', allowEmptyResults: false
        }
        success {
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }
}
