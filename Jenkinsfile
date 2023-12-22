pipeline {
     agent { label 'Jenkins-Agent'}
     tools {
          maven 'maven3'
          jdk 'java17'

     }
     stages{
         stage("Cleanup workspace") {
                steps {
                cleanWs()
                }
         }

         stage("Checkout from SCM") {
                steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/learnDevops1980/register-app.git'
                }
         }

         stage("Build Application") {
                steps {
                 sh "mvn clean package"
                }
         }
         stage("Test Application") {
                steps {
                 sh "mvn test"
                }
         }
         stage("sonarqube analysis"){
           steps {
               script {
		        withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token') { 
                        sh "mvn sonar:sonar"
		        }
	           }
           }
        }
        stage("Quality Gate"){
           steps {
               script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
                }	
            }

        }

     }


}
