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
                // 'mvn test' exécute les tests ET génère automatiquement la couverture JaCoCo
                bat 'mvn test'
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
            // Remplacement de CoberturaPublisher par JacocoPublisher
            jacoco()
        }
        failure {
            mail to: 'admin@esi.ac.ma',
                 subject: "Échec du build",
                 body: "Vérifiez les logs Jenkins."
        }
    }
}
