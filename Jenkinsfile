pipeline {
    agent any
    environment {     
        DOCKERHUB_CREDENTIALS= credentials('dhub')
    }    
    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/momynkulov/cicd-pipeline.git'
            }
        }

        stage('App Build') {
            steps {
                sh 'chmod 777 scripts/build.sh'
                sh 'script scripts/build.sh'
            }
        }

        stage('Tests') {
            steps {
              sh 'script scripts/test.sh'
            }
        }

        stage('Node.js App Build') {
            steps {
                script {
                  sh 'docker build -t momynkul/cicdepamimage .'
                  sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin docker.io'
                  sh 'docker push momynkul/cicdepamimage'
                  sh 'docker logout'
                }
            }
        }
    }
}
