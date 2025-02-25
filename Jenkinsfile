pipeline {
    agent any

    tools {
        maven '3.9.6' // Specify the Maven version installed on Jenkins
    }

    stages {
        stage('Environment Info') {
            steps {
                script {
                    sh 'java -version'
                    sh 'mvn -version'
                }
            }
        }
        stage('Compile') {
            steps {
                script {
                    sh 'mvn clean compile'
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    sh 'mvn test'
                }
            }
        }
        stage('Code Coverage') {
            steps {
                script {
                    sh 'mvn jacoco:report'
                }
            }
        }
        stage('Package') {
            steps {
                script {
                    sh 'mvn package'
                }
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
            junit '**/target/surefire-reports/*.xml'
            recordCoverage(tools: [[parser: 'JACOCO']],
                id: 'jacoco', name: 'JaCoCo Coverage',
                sourceCodeRetention: 'EVERY_BUILD',
                qualityGates: [
                    [threshold: 60.0, metric: 'LINE', baseline: 'PROJECT', unstable: true],
                    [threshold: 60.0, metric: 'BRANCH', baseline: 'PROJECT', unstable: true]
                ]
            )
        }
    }
}
