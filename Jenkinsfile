pipeline {
    agent any
    tools {
        maven 'jenkinsmaven'
    }
    stages {
        stage('Get GitHub') {
            steps {
                git 'https://github.com/Calegria25/AppPipelines.git'

            }
        }
    }
}


