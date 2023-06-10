pipeline {
    agent any

    stages {
        stage('Clonar el Repositorio'){
            steps {
                git branch: 'main', credentialsId: 'git-jenkins', url: 'https://github.com/Alexis-Machado/microproyectos-jenkins.git'
            }
        }
        stage('Construir imagen de Docker'){
            steps {
                script {
                    withCredentials([
                        string(credentialsId: 'MONGO_URL', variable: 'MONGO_URL')
                    ]) {
                        docker.build('jmachado12/microproyectos:v1', '--build-arg MONGO_URL=${MONGO_URL} .')
                    }
                }
            }
        }
        stage('Desplegar contenedores Docker'){
            steps {
                script {
                    withCredentials([
                            string(credentialsId: 'MONGO_URL', variable: 'MONGO_URL')
                    ]) {
                        sh """
                            sed 's|\\${MONGO_URL}|${MONGO_URL}|g' docker-compose.yml > docker-compose-update.yml
                            docker-compose -f docker-compose-update.yml up -d
                        """
                    }
                }
            }
        }
    }
}