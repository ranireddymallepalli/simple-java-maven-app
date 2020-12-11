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
                sh "echo $PATH"
                sh "mvn clean install"
            }
        }
    }
}
