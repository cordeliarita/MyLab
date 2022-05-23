pipeline{
    //Directives
    agent any //
    tools {
        maven 'maven'
    }
    environment{
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getversion()
        Name = readMavenPom().getName()
        GroupId = readMavenPom().getGroupId()
    }
            triggers {
  pollSCM '* * * * *'
}

    stages {
        // Specify various stage with in stages

        // stage 1. Build
        stage ('Build'){
            steps {
                sh 'mvn clean install package'
            }
        }

        // Stage2 : Testing
        stage ('Test'){
            steps {
                echo ' testing......'

            }
        }

        // Stage 3: Publish the artifact to Nexus
    
        stage ('Publish to Nexus'){
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'VinayDevOpsLab', classifier: '', file: 'target/VinayDevOpsLab-0.0.5.war', type: 'war']], credentialsId: 'b14160a2-d076-45d3-98f1-a0c62dc1d196', groupId: 'com.vinaysdevopslab', nexusUrl: '172.20.10.4:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'DominiqueDevOpsLab-Release', version: '0.0.5'
            }
        }
        // Stage3 : Publish the source code to Sonarqube
        stage ('Sonarqube Analysis'){
            steps {
                echo ' Source code published to Sonarqube for SCA......'
                withSonarQubeEnv('sonarqube'){ // You can override the credential to be used
                     sh 'mvn sonar:sonar'
                }

            }
            
        // Stage 4 : Print some information
        stage ('Print Environment variables'){
                    steps {
                        echo "Artifact ID is '${ArtifactId}'"
                        echo "Version is '${Version}'"
                        echo "GroupID is '${GroupId}'"
                        echo "Name is '${Name}'"
                    }
                }

        }

        
        
    }

}