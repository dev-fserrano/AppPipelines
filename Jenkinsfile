pipeline {
    agent any
    tools {
        maven 'jenkinsmaven'
    }
    stages {
        stage('Get GitHub') {
            steps {
                git branch: 'main', url: 'git@github.com:Calegria25/AppPipelines.git'
            }
        }
        stage('Generate artifacts') {
            steps{
                echo 'Genera artefactos'
                sh 'mvn clean install'
            }
        }
        stage('storage artifacts'){
            steps{
                echo 'Almacenar artefactos'
                sh 'chmod 777 /var/jenkins_home/workspace/MiAppPipeline'
                archiveArtifacts artifacts: 'target/*.jar', followSymlinks: false
            }
        }
    }
}







