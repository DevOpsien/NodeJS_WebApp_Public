pipeline {
    agent any
    
    tools{
        nodejs 'nodejs-10'
    }

    stages {
        stage('Git Checkout ') {
            steps {
                git branch: 'main', url: 'https://github.com/DevOpsien/NodeJS_WebApp_Public.git'
            }
        }
        stage('install dependencies ') {
            steps {
                sh "npm install"
            }
			}
        stage('OWASP dependencies ') {
            steps {
                dependencyCheck additionalArguments: '--scan ./ ', odcInstallation: 'DP'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage('docker bulid and push  ') {
            steps {
                script{
                    withDockerRegistry(credentialsId: '0774c1aa-ebc8-48b9-8eb9-3d9990e3bd0e') {
                    sh "docker build -t deamonnodejs ."
                    sh "docker tag deamonnodejs 96445162/nodejs:latest"
                    sh "docker push 96445162/nodejs:latest"

                        }
                    }
				}
			}
		stage('deploy  ') {	
            steps {
                script{
                    withDockerRegistry(credentialsId: '0774c1aa-ebc8-48b9-8eb9-3d9990e3bd0e') {
                        sh "docker run -d --name demo-nodejs -p 8081:8081 96445162/nodejs:latest "
                    }
                }
            }    
        }
    }
}	
