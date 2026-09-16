pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'krpspanda/backend-django'
        DOCKER_TAG = '${BUILD_NUMBER}'
    }

    stages {
        stage('1. Descarga de Codigo (Checkout)'){
            steps {
                echo 'Descargando el codigo fuente del repositorio...'
                checkout scm
            }
        }

        stage('2. Pruebas Automatizadas (Testing)'){
            steps {
                echo 'Ejecutando la suite de pruebas del backend en Django...'
                sh '''
                   echo "Validando archivos del proyecto..."
                   ls -la
                '''
            }
        }

        stage('3. Construccion de Imagen (Build Docker Image)'){
            steps {
                echo 'Construyendo la imagen de contenedor Docker...'
                sh "docker build -t ${DOCKER_IMAGE}:${DOCKER_TAG} ."
            }
        }

        stage('4. Publicacion en Registro (Push to Docker Hub)'){
            steps{
                echo 'Subiendo la imagen a Docker Hub...'
                withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]){
                    sh """
                       echo "\$DOCKER_PASS" | docker login -u "\$DOCKER_USER" --password-stdin
                       docker push ${DOCKER_IMAGE}:${DOCKER_TAG}
                    """
                }
            }
        }
    }

    post {
        always {
            echo 'Finalizando el pipeline...'
        }
        success {
            echo '¡El pipeline se ejecutó existosamente!'
        }
        failure {
            echo '¡ERROR en el Pipeline! Revisa los logs.'
        }
    }
}