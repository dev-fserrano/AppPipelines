pipeline {
    agent any
    tools {
        maven 'jenkinsmaven'
    }
    stages {
        stage('Get GitHub') {
            steps {
                git branch: 'main', url: 'https://github.com/Calegria25/AppPipelines.git'
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
                sh 'chmod 777 $PWD'
                sh 'chmod 777 /var/jenkins_home/workspace'
                archiveArtifacts artifacts: 'target/*.jar', followSymlinks: true , fingerprint: true
            }
        }
        stage('TESTS') {
            steps{
                echo 'Realiza Pruebas'
                sh 'mvn test '
            }
        }
        stage('Sonar Scanner') {
            steps {
                withSonarQubeEnv('sonarenv'){
                    script {
                        def sonarqubeScannerHome = tool name: 'sonar', type: 'hudson.plugins.sonar.SonarRunnerInstallation'
                        withCredentials([string(credentialsId: 'sonar', variable: 'sonarLogin')]) {
                        sh "${sonarqubeScannerHome}/bin/sonar-scanner -e -Dsonar.host.url=http://SonarQube:9000 -Dsonar.login=${sonarLogin} -Dsonar.projectName=mv-maven -Dsonar.projectVersion=${env.BUILD_NUMBER} -Dsonar.projectKey=GS -Dsonar.sources=src/main/java/com/kibernumacademy/miapp -Dsonar.tests=src/test/java/com/kibernumacademy/miapp -Dsonar.language=java -Dsonar.java.binaries=."
                    }
                }
            }

            }
        }
        stage("Quality Gate") {
        steps {
            withS
            timeout(time: 15, unit: 'MINUTES') { // If analysis takes longer than indicated time, then build will be aborted
                waitForQualityGate abortPipeline: true
                script{
                    def qg = waitForQualityGate() // Waiting for analysis to be completed
                    if(qg.status != 'OK'){ // If quality gate was not met, then present error
                        error "Pipeline aborted due to quality gate failure: ${qg.status}"
                    }
                }
            }
        }
    }
}

}





