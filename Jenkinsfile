pipeline{
    environment { 
        registry = "ligmaboys/timesheet" 
        registryCredential = 'dockerhub' 
        dockerImage = 'timesheet' 
    }
  tools {
      maven "maven"
    }
  
    agent any
   
    stages{
        stage(' GIT '){
            steps {
                echo"Apporter mon projet a partir du git"
                git([url: 'https://github.com/candis-D7itinjoace/timesheet-esprit.git', branch: 'main'])

                
            }
        }
        stage('Maven Clean Compile Package'){
            steps {
                
                sh 'mvn clean install -Dmaven.test.skip=true'
            }
            
        }
        stage('Maven Test'){
            steps {
                echo "Lancer les testes Maven "
                
            }
        }

        stage('Construire notre image ') { 

            steps { 

                script { 
                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 
                }

        }
        }
        stage('Deployer notre image') { 

            steps { 

                script { 

                    docker.withRegistry( '', registryCredential ) { 

                        dockerImage.push() 

                    }

                } 

            }
            

        } 
          stage('Deploying the image into a container'){
            steps {
                
                bat "docker run -d --name mycontainer -p 8082:8082 $registry:$BUILD_NUMBER"
            }
        }
       
        
    }
    
}
