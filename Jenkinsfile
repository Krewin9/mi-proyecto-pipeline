pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                script {
                    echo 'Clonando el repositorio y preparando el entorno'
                    git branch: 'main', credentialsId: 'tu-credencial-id', url: 'https://github.com/usuario/nombre-del-repositorio.git'
                }
            }
        }

        stage('Compilar y Probar') {
            steps {
                script {
                    echo 'Compilando y probando el código'
                    sh './gradlew build'  
                }
            }
        }
    }
}
