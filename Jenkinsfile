pipeline {

    agent any

    tools {

        maven "MAVEN"

    }

    stages {

       stage('cloning git repo'){
       
           steps {
               
                git branch: 'master', url: 'https://github.com/viraj777/maven-project.git'

          
                sh "mvn -Dmaven.test.failure.ignore=true clean package"

            }

        }

      stage('pushing artifact to nexus'){

           steps {

                nexusArtifactUploader artifacts: [[artifactId: 'maven-project',
                classifier: '', file: 'target/webapp.war', type: 'war']],
                credentialsId: 'nexus', 
                groupId: 'com.example.maven-project',
                nexusUrl: 'viraj_SNAPSHOT', nexusVersion: 'nexus3',
                protocol: 'http',
                repository: 'viraj_SNAPSHOT',
                version: '1.0-SNAPSHOT'

          }

        }

     }

 }

