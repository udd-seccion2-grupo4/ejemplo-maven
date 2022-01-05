pipeline {
    agent any

    stages {
        stage('Compile') {
            steps {
                script {
                    sh "./mvnw clean compile -e"
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh "./mvnw clean test -e"
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    sh "./mvnw clean package -e"
                }
            }
        }
        stage('Run') {
            steps {
                script {
                    sh "./mvnw spring-boot:run &"
                    sleep 20
                }
            }
        }
        stage('Test Run') {
            steps {
                script {
                   sh 'curl -X GET http://localhost:8081/rest/mscovid/test?msg=testing'
                }
            }
        }
    }
}