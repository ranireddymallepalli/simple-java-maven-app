pipeline {
    agent{
        label 'agent1'
    }
    tools {
        maven 'maven363'
    }
    environment {
        target_user = "ec2-user"
        target_server = "172.31.37.153"
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
        stage("Deploy to Dev") {
            parallel {
                stage('target1'){
                    environment {
                        target_user = "ec2-user"
                        target_server = "172.31.37.153"
                    }
                    steps {
                        echo "Deploying to Dev Environment"
                        sshagent(['maven-cd-key']) {
                            sh "scp -o StrictHostKeyChecking=no target/my-app-1.0-SNAPSHOT.jar $target_user@$target_server:/home/ec2-user"
                        }
                    }
                }
                    stage('target2'){
                    environment {
                        target_user = "ec2-user"
                        target_server = "172.31.29.225"
                    }
                    steps {
                        echo "Deploying to Dev Environemtn"
                        sshagent(['maven-cd-key']) {
                            sh "scp -o StrictHostKeyChecking=no target/my-app-1.0-SNAPSHOT.jar $target_user@$target_server:/home/ec2-user"
                        }
                    }
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
/*
        stage('Deploy'){
            steps {
                echo "Deploying to dev environment"
                sshagent(['maven-cd-key']) {
                    sh "scp -o StrictHostKeyChecking=no target/my-app-1.0-SNAPSHOT.jar $target_user@$target_server:/home/ec2-user"
                }
            }
        }
    }
*/
