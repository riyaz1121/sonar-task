pipeline{
    
    agent any 
    
    stages {
        
        stage('Git Clone'){
            
            steps{
                
                script{
                    
                    git branch: 'main', credentialsId: 'git_credentials', url: 'https://github.com/riyaz1121/sonar-task.git'
                }
            }
        }
    stage('Maven Build'){
        
        steps{
            
            script{
                def mavenHome = tool name: "Maven-3.9.0", type: "maven"
                def mavenCMD = "${mavenHome}/bin/mvn"
                sh "${mavenCMD} clean package"
            }
        }
      }
    stage('SonarQube analysis'){
        
        steps{
                script{
                     withSonarQubeEnv(credentialsId: 'sonar-token') {
                     def mavenHome = tool name: "Maven-3.9.0", type: "maven"
                     def mavenCMD = "${mavenHome}/bin/mvn"
                     sh "${mavenCMD} sonar:sonar"
                    } 
                }
           }
    }
       stage('upload war to nexus'){ 
           
           steps{
                script{
                    
                   nexusArtifactUploader artifacts: [
                       [
                           artifactId: 'pasha-1', 
                           classifier: '', 
                           file: 'target/pasha-1-3.9.5.jar', 
                           type: 'jar'
                           ]
                           ], 
                           credentialsId: 'nexus-credentials', 
                           groupId: 'pasha', 
                           nexusUrl: '3.95.83.239:8081/', 
                           nexusVersion: 'nexus3', 
                           protocol: 'http', 
                           repository: 'pasha', 
                           version: '3.9.5'
               } 
           }
        }
        
    }
}
