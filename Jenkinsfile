pipeline {
    agent {
     label 'python'
     }
    
    environment {
     HARBOR_CREDENTIAL_ID = 'harbor-id'
     REGISTRY = '172.30.102.24:30002'
     HARBOR_NAMESPACE = 'library'
     APP_NAME = 'demo-python-app'
     
     PATH="/var/lib/jenkins/miniconda3/bin:$PATH"
    }
    stages {
         stage('Install') {
            steps {
              container('python') {
                sh 'python --version'
              }
            }
        }
	  stage('Build') {
              steps {
                  container ('python') {
                   echo 'Build python project'
                   sh 'python hello.py'
                }
              }
         }
    
      
        
    
    
        stage(" docker build") {
            steps {
                container ('python') {
                  sh 'docker build -t $REGISTRY/$HARBOR_NAMESPACE/$APP_NAME:10.0.0-SNAPSHOT .' 
       
                }
               
            }
	}
          
          
         	    
	    stage(' push image on harbor') {
          steps {
            container('python') {
                  
                  withCredentials([usernamePassword(passwordVariable: 'HARBOR_PASSWORD', usernameVariable: 'HARBOR_USERNAME', credentialsId: "$HARBOR_CREDENTIAL_ID",)]) {
                                sh 'echo "$HARBOR_PASSWORD" | docker login $REGISTRY -u "$HARBOR_USERNAME" --password-stdin'
                                sh 'docker push $REGISTRY/$HARBOR_NAMESPACE/$APP_NAME:10.0.0-SNAPSHOT'
        }
      }
    }
  }
	
	    stage("Remove the local Docker image") {
      steps {
        container('python') {
          sh '''
            docker image rm $REGISTRY/$HARBOR_NAMESPACE/$APP_NAME:10.0.0-SNAPSHOT
          '''
        }
      }
    }
 }
}
