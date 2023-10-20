pipeline {
    agent {label 'maven-label'}

    tools {
        maven "M3"
    }
    stages {
        stage('prepare') {
            steps {
                checkout scm
            }
        }
        stage('scan') {
            steps {
                withCredentials([string(credentialsId: 'VULNEDO_TOKEN', variable: 'VULNEDO_TOKEN')]) {
                sh "mvn clean verify sonar:sonar \
                        -Dsonar.projectKey=vulnado \
                        -Dsonar.projectName='vulnado' \
                        -Dsonar.host.url=http://172.31.11.244:9000 \
                        -Dsonar.token='${VULNEDO_TOKEN}'"
                }
            }
        }
        stage('Build') {
            steps {
                sh "mvn -Dmaven.test.failure.ignore=true -s settings.xml clean package"
            }

            post {
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
    }
}
