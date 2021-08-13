pipeline {
    agent {
      label 'linux'
    }
    
    tools {
      maven 'mvn_3.8.1'
        dockerTool 'docker'
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
        stage('Check docker image') {
            steps {
               sh '''docker image ls
                     docker image build . -f Dockerfile -t madhavaagrisutra/devops:latest
                     docker image ls'''
            }
        }
         stage('Push docker image') {
            steps {
               sh '''docker login -u ${dockeruser} -p ${dockerpassword}
                    docker push madhavaagrisutra/devops:latest'''
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
