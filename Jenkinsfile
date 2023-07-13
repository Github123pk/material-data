pipeline {
    agent any
    stages {
        
        stage('checkout'){
		    steps {
            checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/psabyasa/Practice.git']])
        }
		}
        stage('Build') {
            steps {
                bat "mvn -Dmaven.test.failure.ignore=true clean package"

                // To run Maven on a Windows agent, use
                // bat "mvn -Dmaven.test.failure.ignore=true clean package"
            }

            post {
                // If Maven was able to run the tests, even if some of the test
                // failed, record the test results and archive the jar file.
                success {
                    junit '**/target/surefire-reports/TEST-*.xml'
                    archiveArtifacts 'target/*.jar'
                }
            }
        }
        stage('DockerBuild'){
            steps{
                bat "docker build -f Dockerfile ."
                
            }
        }
    }
}

