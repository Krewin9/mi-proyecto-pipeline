pipeline {
    agent any

    environment {
        // No hay variables de entorno definidas por el momento
    }

    stages {
        stage('Declarative: Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Integrar a Desarrollo') {
            steps {
                script {
                    echo 'Integrando cambios a la rama de desarrollo'
                    bat 'git pull origin desarrollo'  // Cambio de sh a bat
                }
            }
        }

        stage('Fusionar a Pruebas') {
            steps {
                script {
                    echo 'Fusionando cambios a la rama de pruebas'
                    bat 'git checkout pruebas'
                    bat 'git merge desarrollo'
                }
            }
        }

        stage('Desplegar en Pruebas') {
            steps {
                script {
                    echo 'Desplegando en servidor de pruebas'
                    // Aquí agregar los comandos para desplegar en el servidor de pruebas
                    bat 'echo Desplegando en pruebas...'
                }
            }
        }

        stage('Respaldo de Producción') {
            steps {
                script {
                    echo 'Realizando respaldo del directorio de producción'
                    bat 'xcopy /E /I /Y "C:\\Produccion" "C:\\Backup\\ProduccionBackup"'
                }
            }
        }

        stage('Fusionar a Producción') {
            steps {
                script {
                    echo 'Fusionando cambios a la rama de producción'
                    bat 'git checkout produccion'
                    bat 'git merge pruebas'
                }
            }
        }

        stage('Desplegar a Producción') {
            steps {
                script {
                    echo 'Desplegando a producción'
                    bat 'xcopy /E /I /Y "C:\\Backup\\ProduccionBackup" "C:\\Produccion"'
                    // Aquí puedes agregar más comandos de despliegue
                }
            }
        }

        stage('Restaurar en caso de error') {
            steps {
                script {
                    echo 'Restaurando desde el respaldo en caso de error'
                    bat 'xcopy /E /I /Y "C:\\Backup\\ProduccionBackup" "C:\\Produccion"'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline terminado.'
        }
        success {
            echo 'Pipeline ejecutado correctamente.'
        }
        failure {
            echo 'Pipeline fallido, restaurando producción.'
            // Si necesitas restaurar producción en caso de fallo
            bat 'xcopy /E /I /Y "C:\\Backup\\ProduccionBackup" "C:\\Produccion"'
        }
    }
}
