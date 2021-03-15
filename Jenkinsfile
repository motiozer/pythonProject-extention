pipeline {
    environment {
    registry = "motiozer/project-extention"
    registryCredential = 'docker_hub'
    dockerImage = ''
    }
    agent any
    stages {
        stage('checkout') {
            steps {
                script {
                    properties([pipelineTriggers([pollSCM('* * * * *')])])
                }
                git 'https://github.com/motiozer/pythonProject-extention.git'
            }
        }
        stage('run rest server') {
            steps {
                bat 'start /min python rest_app.py'

                }
        }
				stage('run backend_testing') {
            steps {
					bat 'python backend_testing.py'

                }
        }

				stage('clean_environemnt') {
            steps {
					bat 'python clean_environment.py'

                }
        }
               stage('build and push image') {
            steps {
                script {
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                    docker.withRegistry('', registryCredential) {
                    dockerImage.push()
           }
         }
       }
    }
}