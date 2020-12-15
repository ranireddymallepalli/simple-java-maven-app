pipeline {
    agent{
        label 'agent1'
    }
    tools {
        maven 'maven363'
    }
    options {
       timeout(10)
       buildDiscarder logRotator(artifactDaysToKeepStr: '', artifactNumToKeepStr: '', daysToKeepStr: '5', numToKeepStr: '5')
    } 

    stages {
        stage('Checkout'){
            steps {
               checkout scm
            }
        }
        stage('Build'){
            steps {
                sh "mvn clean install -DskipTests"
            }
        }
        stage('Test'){
            steps{
                sh "mvn test"
            }
        }
        stage('Deploy'){
            steps {
                 echo "Deploying to dev environment"
                 sshagent(['maven-cd-key']) {
                     sh "scp target/my-app-1.0.SNAPSHOT-jar ece-user@172.31.37.153:/home/ec2-user"
                    }
            }
        }
    }
    post {
         always{
            deleteDir()
        }
        failure{
            echo "sednmail -s Maven Job failed recipients@mycompany.com"
        }
        success{
            echo "The job is successful"
        }
    }
}

