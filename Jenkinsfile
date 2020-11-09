pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/nicosoutn2014/spring-petclinic.git'

                sh 'chmod +x gradlew'
                sh './gradlew build'
            }
        }

        stage('Test') {
            steps {
                sh './gradlew test'
            }
        }

        stage('Validate') {
            steps {
                sh './gradlew check'
            }
        }

        stage('Analyze') {
            steps {
                // corre en docker
                sh './gradlew sonarqube -Dsonar.host.url=http://sonarqube:9000'
                // jacoco
                sh './gradlew -i test jacocoTestReport'
            }
        }

        stage('Deploy') {
            steps {
                sh 'chmod +x mvnw'
                sh './mvnw spring-boot:run -Dcheckstyle.skip'
            }
        }
    }
}
