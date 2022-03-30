# jenkins-docker-agent
Jenkins Docker Agent

Steps :

1. Install Jenkins and do initital setup
2. Install docker and give permission on "/var/run/docker.sock" to 777
3. Install Docker Pipeline plugin /sonarqube plugin on jenkins
4. Install sonarqube server
    - Generate sonar token
    - Create webhook for jenkins

5. Add the sonar token as a secret string and Add SonarQube servers with in jenkins.
