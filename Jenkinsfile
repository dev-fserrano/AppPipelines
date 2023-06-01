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
    //     stage("Quality Gate") {
    //     steps {
    //         timeout(time: 15, unit: 'MINUTES') { 
    //             waitForQualityGate abortPipeline: true
    //             script{
    //                 def qg = waitForQualityGate() 
    //                 if(qg.status != 'OK'){ 
    //                     error "Pipeline aborted due to quality gate failure: ${qg.status}"
    //                 }
    //             }
    //         }
    //     }
    // }
        stage('nexus') {
        steps {
            script {
                  dir("target"){
                    def pom = readMavenPom file: "pom.xml"
                                nexusArtifactUploader(
                                    nexusVersion: 'nexus3',
                                    protocol: 'http',
                                    nexusUrl: 'nexus:8081',
                                    groupId: pom.groupId,
                                    version: pom.version,
                                    repository: 'maven-snapshots',
                                    credentialsId: 'nexus',
                                    artifacts: [
                                        [artifactId: pom.artifactId,
                                        classifier: '',
                                        file: 'MiApp-0.0.1-SNAPSHOT.jar',
                                        type: pom.packaging]
                                    ]
                                )
                            }
            }
        }
    }
}

}





