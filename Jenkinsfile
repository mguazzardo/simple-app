pipeline {
    agent any
    tools {
        maven 'maven 3.6.0'
    }
    options {
        buildDiscarder logRotator(daysToKeepStr: '5', numToKeepStr: '7')
    }
    stages{
        stage('Build'){
            steps{
                 sh script: 'mvn clean package'
                 archiveArtifacts artifacts: 'target/*.war', onlyIfSuccessful: true
            }
        }
        stage('Upload War To Nexus'){
            steps{
                script{

                    def mavenPom = readMavenPom file: 'pom.xml'
                    def nexusRepoName = mavenPom.version.endsWith("SNAPSHOT") ? "simpleapp-snapshot" : "simpleapp-release"
                    nexusArtifactUploader artifacts: [
                        [
                            artifactId: 'simple-app', 
                            classifier: '', 
                            file: "target/simple-app-${mavenPom.version}.war", 
                            type: 'war'
                        ],
                        [
                            artifactId: 'prueba-dos', 
                            classifier: '', 
                            file: "prueba.txt", 
                            type: 'txt'
                        ]
                        
                    ], 
                    // las credenciales, definirlas 
                    // groupid, no se si influye
                    // NexusURL, super importante
                    // Protocolo, si obvio
                    credentialsId: 'credentialsID', 
                    groupId: 'in.javahome', 
                    nexusUrl: '192.168.0.190:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'repository-example', 
                    version: "${mavenPom.version}"
                    }
            }
        }
    }
}
