pipeline {
    agent any 
    
    environment {
        // Le chemin vers votre Java 21
        JAVA_HOME = "C:/Program Files/Java/jdk-21.0.11"
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Récupération du code source...'
                git branch: 'main', url: 'https://github.com/O-Herbiet/SpringPetClinic'
            }
        }

        stage('Build & Tests') {
            steps {
                echo 'Compilation et exécution des tests unitaires...'
                // L'arme secrète : -DforkCount=0 tue dans l'œuf le bug du "ForkStarter"
                bat 'mvnw.cmd clean test -Dmaven.repo.local=.m2/repository -DforkCount=0'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Package') {
            steps {
                echo 'Création de l\'archive de l\'application (.jar)...'
                bat 'mvnw.cmd package -DskipTests -Dmaven.repo.local=.m2/repository'
            }
        }
    }

    post {
        success {
            echo ' Le build est un succès !'
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
        failure {
            echo 'Le build a échoué. Vérifiez les logs.'
        }
    }
}
