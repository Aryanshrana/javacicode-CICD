pipeline {

    agent any

    stages {
        stage("GIT Checkout") {
            steps {
                git 'https://github.com/Aryanshrana/javacicode-CICD.git'
            }
        }

        stage("UNIT TESTING") {
            steps {
                sh 'mvn test'
            }
        }

        stage("INTEGRATION TESTING") {
            steps {
                sh 'mvn verify -DskipUnitTesting'
            }
        }

        stage("BUILD") {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('STATIC CODE ANALYSIS') {
            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonarqubeToken') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }

        stage('QUALITY GATE STATUS') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonarqubeToken'
                }
            }
        }

        stage('Upload Artifacts to Nexus'){
            
            steps{

                script{

                    nexusArtifactUploader artifacts: 
                [
                    [
                    artifactId: 'springboot',
                    classifier: '', 
                    file: 'target/UPES.jar', 
                    type: 'jar'
                    ]
                ], 
                    credentialsId: 'nexus', 
                    groupId: 'com.example', 
                    nexusUrl: '13.127.219.52:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'Java-released', 
                    version: '1.0.0'
                }
            }
        }

    }
}