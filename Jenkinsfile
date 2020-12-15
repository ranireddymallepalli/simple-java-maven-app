pipeline {
    agent{
        label 'agent1'
    }
    tools {
        maven 'maven363'
    }
    stages {
        stage('Checkout'){
            steps {
               checkout scm
            }
        }
        stage('Build'){
            steps {
                sh "mvn --version"
                sh "mvn clean install"
            }
        }
    }
}

