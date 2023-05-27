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
    }
}


