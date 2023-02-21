pipeline {

    agent any

    tools {

        maven "MAVEN"

    }
    
    environment{

       ArtifactID = readMavenPom().getArtifactId()
       GroupID = readMavenPom().getGroupId()
       Version = readMavenPom().getVersion()

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

                script{

                def RepoName = Version.endsWith("SNAPSHOT") ? "viraj_SNAPSHOT" : "viraj_RELEASE"

                nexusArtifactUploader artifacts: [[artifactId: "${ArtifactID}",
                classifier: '', file: 'webapp/target/webapp.war', type: 'war']],
                credentialsId: 'nexus', 
                groupId: "{GroupID}",
                nexusUrl: '172.31.19.161:8081', nexusVersion: 'nexus3',
                protocol: 'http',
                repository: "${RepoName}",
                version: "${Version}"

            }

          }

        }

     }

 }

