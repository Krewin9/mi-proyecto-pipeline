pipeline {
    agent any
    environment {
        GIT_CREDENTIALS = 'git_credentials' // Nombre de tus credenciales de Git en Jenkins
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
                    // Actualizar todas las ramas remotas
                    bat 'git fetch --all'
                    // Realizar un pull de la rama de desarrollo
                    bat 'git pull origin desarrollo'
                }
            }
        }

        stage('Fusionar a Pruebas') {
            steps {
                script {
                    echo 'Fusionando cambios a la rama de pruebas'
                    // Asegurarse de que estamos en la rama correcta
                    bat 'git checkout pruebas'
                    // Realizar el merge con la rama remota 'desarrollo'
                    bat 'git merge origin/desarrollo'  // Usar la referencia remota 'origin/desarrollo'
                }
            }
        }

        stage('Desplegar en Pruebas') {
            when {
                expression {
                    return currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                script {
                    echo 'Desplegando en pruebas...'
                    // Aquí iría el comando de despliegue, por ejemplo:
                    // bat 'deploy_command'
                }
            }
        }

        stage('Respaldo de Producción') {
            when {
                expression {
                    return currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                script {
                    echo 'Realizando respaldo de producción...'
                    // Aquí iría el comando de respaldo
                    // bat 'backup_command'
                }
            }
        }

        stage('Fusionar a Producción') {
            when {
                expression {
                    return currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                script {
                    echo 'Fusionando cambios a la rama de producción'
                    // Cambiar a la rama de producción
                    bat 'git checkout produccion'
                    // Realizar el merge con la rama de pruebas
                    bat 'git merge origin/pruebas'
                }
            }
        }

        stage('Desplegar a Producción') {
            when {
                expression {
                    return currentBuild.result == null || currentBuild.result == 'SUCCESS'
                }
            }
            steps {
                script {
                    echo 'Desplegando en producción...'
                    // Aquí iría el comando de despliegue, por ejemplo:
                    // bat 'deploy_production_command'
                }
            }
        }

        stage('Restaurar en caso de error') {
            when {
                expression {
                    return currentBuild.result == 'FAILURE'
                }
            }
            steps {
                script {
                    echo 'Restaurando producción debido a un error...'
                    // El comando para restaurar el backup
                    bat 'xcopy /E /I /Y "C:\\Backup\\ProduccionBackup" "C:\\Produccion"'
                }
            }
        }

        stage('Declarative: Post Actions') {
            steps {
                echo 'Pipeline terminado.'
                script {
                    if (currentBuild.result == 'FAILURE') {
                        echo 'Pipeline fallido, restaurando producción.'
                    }
                }
            }
        }
    }
}
