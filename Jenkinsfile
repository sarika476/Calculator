pipeline{
    agent any
    environment {
        dockerImage='calc'
        registry = "sarika476/scientific_calculator"
        registryCredential = 'docker-login'
    }
    stages {
        stage('Clone GitHub Repository') {
            steps {
                git url: 'https://github.com/sarika476/Calculator.Calculator.git', branch: 'master',
                 credentialsId: 'f3292e40-d005-4e9b-a4b9-8c6eeaff6585'
            }
        }
        stage('Test'){
            steps {
                sh 'mvn clean test'
            }
        }
        stage('Build'){
            steps {
                sh 'mvn install'
            }
        }
        stage('Building our image') {
            steps{

                script {
                    dockerImage = docker.build registry
                }


            }
        }
        stage('Deploy our image') {
            steps{
                script {
                    docker.withRegistry( '', registryCredential ) {
                        dockerImage.push()
                    }
                }
            }
        }
        stage('Ansible Deploy') {
             steps {
                  ansiblePlaybook colorized: true, disableHostKeyChecking: true, installation: 'Ansible', inventory: 'inventory', playbook: 'deploy.yml'
             }
        }
    }
    post {
        always {
            echo 'I am done with or without failure'
            deleteDir() //cleaning
        }
    }
}