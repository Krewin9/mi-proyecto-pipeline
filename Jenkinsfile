pipeline {
    agent any
    environment {
        BACKUP_DIR = "/ruta/a/respaldo"  // Cambia la ruta al directorio de respaldo
        PROD_DIR = "/ruta/a/produccion"  // Cambia la ruta al directorio de producción
    }
    stages {
        stage('Integrar a Desarrollo') {
            steps {
                script {
                    echo 'Integrando cambios a la rama de desarrollo'
                    sh 'git checkout desarrollo'
                    sh 'git pull origin desarrollo'
                    sh 'git merge origin/mi-rama'
                    sh 'git push origin desarrollo'
                }
            }
        }

        stage('Fusionar a Pruebas') {
            steps {
                script {
                    echo 'Fusionando cambios de desarrollo a pruebas'
                    sh 'git checkout pruebas'
                    sh 'git pull origin pruebas'
                    sh 'git merge origin/desarrollo'
                    sh 'git push origin pruebas'
                }
            }
        }

        stage('Desplegar en Pruebas') {
            steps {
                script {
                    echo 'Desplegando en servidor de pruebas'
                    sh 'ssh usuario@servidor_pruebas "cd /ruta/del/proyecto && git pull origin pruebas && docker-compose up -d"'
                }
            }
        }

        stage('Respaldo de Producción') {
            steps {
                script {
                    echo 'Realizando respaldo de la producción'
                    sh 'ssh usuario@servidor_produccion "cp -r $PROD_DIR $BACKUP_DIR/produccion_backup_$(date +%Y%m%d%H%M)"'
                }
            }
        }

        stage('Fusionar a Producción') {
            steps {
                script {
                    echo 'Fusionando cambios de pruebas a producción'
                    sh 'git checkout produccion'
                    sh 'git pull origin produccion'
                    sh 'git merge origin/pruebas'
                    sh 'git push origin produccion'
                }
            }
        }

        stage('Desplegar a Producción') {
            steps {
                script {
                    echo 'Desplegando cambios a producción'
                    sh 'scp -r /ruta/del/proyecto usuario@servidor_produccion:/ruta/de/produccion'
                }
            }
        }

        stage('Restaurar en caso de error') {
            steps {
                script {
                    echo 'Restaurando desde el respaldo si hay un error'
                    sh 'ssh usuario@servidor_produccion "cp -r $BACKUP_DIR/produccion_backup_$(date +%Y%m%d%H%M) $PROD_DIR"'
                }
            }
        }
    }
}
