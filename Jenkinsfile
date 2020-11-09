pipeline {
    agent any
    
    tools {
        gradle 'gradle'
    }
    stages {
       stage('Clone') {
           steps {
               git 'https://github.com/nicosoutn2014/spring-petclinic.git'
           }
       }
       stage('Clean & Build') {
            steps {
                sh "chmod +x gradlew"
                sh './gradlew build'
            }
        }
        stage('Test') {
            steps {
                script {
                    sh './gradlew test'
                }
            }
        }
        stage("Validate") {
            steps {
                sh "./gradlew check"
            }
        }

        stage("Analyze") {
            steps {
                // sonarqube corriendo en docker
                sh "./gradlew sonarqube -Dsonar.host.url=http://sonarqube:9000"
                // jacoco
                sh "./gradlew -i test jacocoTestReport"
            }
        }

        stage("Deploy") {
            steps {
                sh "chmod +x mvnw"
                sh "./mvnw spring-boot:run"
            }
        }
    }
}