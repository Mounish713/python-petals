pipeline {
    agent {
     label 'python'
   }
    environment {
    HARBOR_CREDENTIAL_ID = 'harbor-id'
    REGISTRY = '172.30.102.24:30002'
    HARBOR_NAMESPACE = 'library'
    APP_NAME = 'demo-python-lang-app'
    }
    
    stages {
        stage('Build') {
            agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                sh 'python -m py_compile sources/add2vals.py sources/calc.py'
                stash(name: 'compiled-results', includes: 'sources/*.py*')
            }
        }
    }
   
       
    
}
