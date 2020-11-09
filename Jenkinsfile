pipeline {
    agent any

    //tools {
        // Install the Maven version configured as "M3" and add it to the path.
        //maven "M3"
    //}

    stages {
        stage('Build') {
            steps {
                // Get some code from a GitHub repository
                git 'https://github.com/gabriell24/six-temas.git'

                //sh "chmod +x gradlew" ya lo hice en git
                sh "./gradlew build"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }
        }

        stage("Test") {
            steps {
                // ejecutar con gradle la batería de pruebas y generar un reporte de resultado de las mismas
                sh "./gradlew test"
            }
        }

        stage("Validate") {
            steps {
                sh "./gradlew check"
            }
        }

        stage("Analyze") {
            steps {
                // esta preparado para docker
                sh "./gradlew sonarqube -Dsonar.host.url=http://sonarqube:9000"
                // Step JaCoCo
                sh "./gradlew -i test jacocoTestReport"
            }
        }

        stage("Deploy") {
            steps {
                //sh "chmod +x mvnw" ya lo hice en git
                sh "./mvnw spring-boot:run"
                // acá habría que settear las credenciales de heroku, y subir el war
            }
        }
    }
}