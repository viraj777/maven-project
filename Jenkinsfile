pipeline {

    agent any

    tools {

        maven "maven"

    }

    stages {

        stage('Ignoring git SSL'){
            steps{

                 git([url: 'https://github.com/viraj777/node-todo-cicd.git', branch: 'master', disableCertificateValidation: true])

            }

          }
        stage('Build') {
            steps {
               
                git branch: 'master', url: 'https://github.com/viraj777/maven-project.git'

          
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

            }

        }
     }
 }

