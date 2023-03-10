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
                groupId: "${GroupID}",
                nexusUrl: '172.31.19.161:8081',
                nexusVersion: 'nexus3',
                protocol: 'http',
                repository: "${RepoName}",
                version: "${Version}"

            }

          }

       }

      stage('Pulling latest release artifact from nexus and creating dockerfile') {

         steps {

             sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible-Controller',
             transfers: [sshTransfer(cleanRemote: false,
             excludes: '',
             execCommand: 'ansible-playbook /home/cloud_user/project/makefile.yaml -i /home/cloud_user/project/inventory.txt',
             execTimeout: 120000,
             flatten: false,
             makeEmptyDirs: false,
             noDefaultExcludes: false,
             patternSeparator: '[, ]+',
             remoteDirectory: '',
             remoteDirectorySDF: false,
             removePrefix: '', sourceFiles: '')],
             usePromotionTimestamp: false,
             useWorkspaceInPromotion: false,
             verbose: false)])

           }
        
         }

      stage('pushing image to dockerhub') {
 
          agent {

             label 'agent-1'

             }

         steps {

             withCredentials([usernamePassword(credentialsId: 'Dockerhub', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]){
                    sh "docker login -u ${env.USERNAME} -p ${env.PASSWORD}"
                    sh "docker push virajthorat776/javaapp"
                    }
              }

           }

     stage('Creating docker-compose.yaml and then initializing empty swarm on remote host') {

         steps {

             sshPublisher(publishers: [sshPublisherDesc(configName: 'Ansible-Controller',
             transfers: [sshTransfer(cleanRemote: false,
             excludes: '',
             execCommand: 'ansible-playbook /home/cloud_user/project/build.yaml -i /home/cloud_user/project/inventory.txt',
             execTimeout: 120000,
             flatten: false,
             makeEmptyDirs: false,
             noDefaultExcludes: false,
             patternSeparator: '[, ]+',
             remoteDirectory: '',
             remoteDirectorySDF: false,
             removePrefix: '', sourceFiles: '')],
             usePromotionTimestamp: false,
             useWorkspaceInPromotion: false,
             verbose: false)])

           }

         }



     }

 }

