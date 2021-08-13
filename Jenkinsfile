pipeline {
    agent {
      label 'linux'
    }
    tools {
      maven 'mvn_3.8.1'
    }

    stages {
        stage('Git clone'){
            steps {
               git credentialsId: 'GITHUB-CREDS', url: 'https://github.com/kul-samples/java_sample_webapp.git'
            }
        }
        stage('Build package') {
            steps {
              sh 'mvn clean package'
            }
            post{
              always {
                archive 'target/devops.war'
              }
            }
        }
        stage('How are you?') {
            steps {
                echo 'How are you?'
            }
        }
        stage ('SAST'){
          parallel{
            stage ('sonar-scan'){
              steps {
                echo 'Running Sonar Scan'
              }
            }
            stage ('synk-scan'){
              steps {
                echo 'Synk scan'
              }
            }
            stage ('DC'){
              steps {
                echo 'Running Dependency Check'
              }
            }
          }
        }
    }
}
