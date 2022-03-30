pipeline {
    agent none
    stages {
        stage('checkout code'){
            agent any
            steps{
                    git branch: 'master', url: 'https://github.com/cloudtechmasters/springboot-maven-course-micro-svc.git'
                }
        }
        stage('Build') { 
            agent {
                docker {
                    image 'maven:3.8.1-adoptopenjdk-11' 
                    args '-v /root/.m2:/root/.m2' 
                    args '-u root --privileged -v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps {
                sh 'mvn clean install -f pom.xml' 
            }
        }
        stage('sonar scanner'){
            agent {
                docker {
                    /*
                    * Reuse the workspace on the agent defined at top-level of
                    * Pipeline, but run inside a container.
                    */
                    reuseNode true
                    image 'sonarsource/sonar-scanner-cli'
                    args '-u root --privileged -v /var/run/docker.sock:/var/run/docker.sock'
                }
            }
            steps{
            script{
                    withSonarQubeEnv(installationName: 'sonar-scanner', credentialsId: 'sonar-token') {
                        sh 'sonar-scanner'
                    }

                    timeout(time: 1, unit: 'HOURS') {
                      def qg = waitForQualityGate()
                      if (qg.status != 'OK') {
                           error "Pipeline aborted due to quality gate failure: ${qg.status}"
                      }
                    }

                }  
            }
        }
    }
}
