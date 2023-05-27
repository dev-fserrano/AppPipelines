pipeline {
    agent any
    tools {
        maven 'jenkinsmaven'
    }
    stages {
        stage('Get GitHub') {
            steps {
                git 'git@github.com:Calegria25/AppPipelines.git'

            }
        }
    }
}


