pipeline {
    agent { label 'SPC'}
    triggers {
        pollSCM ('* * * * *')
    }
    parameters {
        string (name: 'BRANCH', defaultValue: 'master', description: 'Git branch to build')
    }
    environment {
        CI_ENV = 'DEV'
        GIT_URL = 'https://github.com/sandeepvikram/simple-java-maven-app.git'
    }
    options {
        retry(3)
    }
    stages {
        stage('scm') {
            steps {
                git branch: "${params.BRANCH}", url: "${env.GIT_URL}"
                echo env.CI_ENV
            }
        }
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
    }
    post {
        success {
            echo 'The code ran good!'
        }
        failure {
            echo 'The code has an error'
        }
    }
}
