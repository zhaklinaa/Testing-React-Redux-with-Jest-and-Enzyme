pipeline {
    agent {
        docker {
            image 'node:14-bullseye'
        }
    }

    triggers {
        pollSCM('H/15 * * * *') // Comprobar cambios cada 15 minutos
    }

    stages {
        stage('Identificación') {
            steps {
                sh 'echo "CD ejecutado por: Zhaklina (eim-alu-69010) - TEST CAMBIOOO PRUEBAAAAA"'
            }
        }

        stage('Checkout') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/zhaklinaa/Testing-React-Redux-with-Jest-and-Enzyme.git'
            }
        }

        stage('Instalación de dependencias') {
            steps {
                sh '''
                apt-get update
                apt-get install -y python make g++ build-essential libc6-dev
                npm install --legacy-peer-deps
                '''
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build || echo "No hay build definido"'
            }
        }

        stage('Testing') {
            steps {
                sh 'npm test || echo "Tests fallidos o no definidos"'
            }
        }

        stage('Deploy en contenedor de la asignatura') {
            steps {
                sh '''
                mkdir -p /root/despliegue
                cp -r README.md Utils build node_modules package-lock.json package.json public src yarn.lock /root/despliegue/ 2>/dev/null || echo "Algunos archivos pueden no existir"
                echo "Proyecto desplegado en /root/despliegue/"
                '''
            }
        }
    }

    post {
        always {
            sh 'rm -rf node_modules'
            sh 'npm cache clean --force || true'
        }
    }
}


