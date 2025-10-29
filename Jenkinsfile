pipeline {
    agent any

    tools {
        // Installe les outils configurés dans Jenkins
        jdk 'JDK_17'
        maven 'Maven_3_9'
    }

    stages {
        stage('Build Maven') {
            steps {
                // Récupération du code depuis GitHub
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/taha362150/DevOps-Automation-Jenkins-Pipeline-']])

                // Construction du projet avec Maven
                bat 'mvn clean package'
            }
        }

        stage('Build Docker image') {
            steps {
                // Construction de l’image Docker
                bat 'docker build -t taha362150/devops-integration .'
            }
        }

        stage('Push image to Docker Hub') {
            steps {
                // Connexion et push de l’image Docker
                withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'DOCKERHUB_PWD')]) {
                    bat 'docker login -u taha362150 -p %DOCKERHUB_PWD%'
                    bat 'docker push taha362150/devops-integration'
                    bat 'docker logout'
                }
            }
        }
    }
}