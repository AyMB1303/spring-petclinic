pipeline {
    agent any

    tools {
        maven 'Maven-Local'
        jdk 'JDK-Local'
    }

    stages {
        stage('Compilation') {
            steps {
                bat 'mvn compile'
            }
        }

        stage('Tests & Couverture') {
            steps {
                bat 'mvn test'
                bat 'mvn cobertura:cobertura'
            }
            post {
                success {
                    junit 'target/surefire-reports/**/*.xml'
                }
            }
        }

        stage('Documentation') {
            steps {
                bat 'mvn site'
            }
        }

        stage('Packaging') {
            steps {
                bat 'mvn package -DskipTests'
            }
        }
    }

    post {
        always {
            step([$class: 'CoberturaPublisher', coberturaReportFile: 'target/site/cobertura/coverage.xml'])
        }
        failure {
            mail to: 'admin@esi.ac.ma',
                 subject: "Échec du build",
                 body: "Vérifiez les logs Jenkins."
        }
    }
}
