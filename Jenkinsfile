pipeline {
    agent {
        label 'linux'
        }
    tools {
        maven 'maven_3.8.1'
        }
    stages {
        
        stage ('Checkout Java Code'){
            steps{
              git branch: 'master', credentialsId: 'GITHUB-CREDS', url: 'https://github.com/kul-samples/java_sample_webapp.git'
            }
        }
        stage('Build Package') {
            steps {
                sh 'mvn clean package'
                  }
            post {
                always {
                     archive 'target/devops.war'
                       }
                 }
         }
        stage('Docker version?') {
            steps {
                sh 'docker --version'
            }
        }
        stage('create docker image') {
            steps {
                sh '''docker image ls 
                    docker image build .  -f Dockerfile -t shubhamhit/devops:latest
                    docker image ls'''
                  }
        }
        stage('push docker image at Dokcerhub') {
            steps {
                sh 'docker push shubhamhit/devops:latest'
                  }
        }
        stage('create docker Container') {
            steps {
                sh '''docker ps -a
                    docker container run -d -p 8080:8080 --name c1 --hostname c1 shubhamhit/devops:latest
                    docker ps -a'''
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
