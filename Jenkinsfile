pipeline {
    agent { label  'maven-label' }

    tools {
        maven "M3"
    }

    stages {
        stage('Source') {
            steps {             
                checkout scm
            }
        }
        stage('SonarScanner'){
            steps {
                sh "mvn clean verify sonar:sonar \
  -Dsonar.projectKey=vulnado \
  -Dsonar.projectName=vulnado \
  -Dsonar.host.url=http://13.59.208.70:9000 \
  -Dsonar.token=sqp_d748ee96976aefc33dd95cd5174c9849105445c0"
            //  withCredentials([string(credentialsId: 'SONAR_TOKEN', variable: 'SONAR_TOKEN')]) {
            //       sh "mvn verify sonar:sonar -Dsonar.projectKey=devsecopsapp  -Dsonar.host.url=http://172.31.16.115:9000/  -Dsonar.login=sqp_d748ee96976aefc33dd95cd5174c9849105445c0 -Dsonar.projectName=devsecopsapp"
            //     }
            }
        }
        stage('buildandupload'){
            steps {
                withCredentials([string(credentialsId: 'NEXUS_PASSWORD', variable: 'NEXUS_PASSWORD')]) {
                sh 'sed  -i "s/passvar/$NEXUS_PASSWORD/g" settings.xml'   
                sh "mvn -Dmaven.test.failure.ignore=true -s settings.xml clean package"
                }
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
