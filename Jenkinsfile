pipeline {
    
    agent any
    
    tools {
        // Me asegura que al entorno que vaya, que tenga el maven para ejecutarlo
        maven 'mi maven pa compilar'
    }
    
    triggers {
        pollSCM '* * * * *'
    }
    
    stages {
        stage('Compilar') {
            steps {
                echo 'Pedir a maven que compile'
                sh 'mvn compile'
            }
        }
        stage('Pruebas') {
            steps {
                echo 'Pedir a maven que ejecute las pruebas unitarias'
                sh 'mvn test'
            }
            post {
                always{
                    echo 'Guardar los informes de pruebas'
                    junit allowEmptyResults: true, testResults: 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Empaquetar') {
            steps {
                echo 'Pedir a maven que empaquete'
                sh 'mvn package'
            }
            post {
                success{
                    echo 'Guardar el archivo WAR'
                    archiveArtifacts artifacts: 'target/*.war', followSymlinks: false
                }
            }
        }
    }
}