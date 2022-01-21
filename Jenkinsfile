pipeline {
    agent any

    stages {
        stage('Compile') {
            steps {
                script {
                    sh './mvnw clean compile -e'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh './mvnw clean test -e'
                }
            }
        }
        stage('Build') {
            steps {
                script {
                    sh './mvnw clean package -e'
                }
            }
        }
        stage('SonarQube') {
            steps {
                script {
                    def scannerHome = tool 'sonar-scanner';
                    withSonarQubeEnv('docker-compose-sonarqube') {
                      sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=ejemplo-maven -Dsonar.sources=src -Dsonar.java.binaries=build"
                    }
                }
            }
        }
        stage('uploadNexus') {
            steps {
                nexusPublisher nexusInstanceId: 'nexus3-docker',
                nexusRepositoryId: 'test-repo',
                packages: [
                    [
                        $class: 'MavenPackage',
                        mavenAssetList: [
                            [classifier: '', extension: '', filePath: 'build/DevOpsUsach2020-0.0.1.jar']
                        ],
                        mavenCoordinate: [
                            artifactId: 'DevOpsUsach2020',
                            groupId: 'com.devopsusach2020',
                            packaging: 'jar',
                            version: '0.0.1'
                        ]
                    ]
                ]
            }
        }
    }
}