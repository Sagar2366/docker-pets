pipeline {

    agent none
    options {
        skipStagesAfterUnstable()
    }
    
    stages {
        stage('Build') {
           agent {
                docker {
                    image 'python:2-alpine'
                }
            }
            steps {
                echo "Building application started ...."
                sh 'pip install flask==0.10.1 python-consul'
                sh 'python app.py & python admin.py'
                echo 'Application build completed'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing'
            }
        }
        
       stage('Sanity check') {
            steps {
                input "Does the staging environment look ok?"
            }
        }
        
        stage('Deploy') {
            steps {
                echo 'Deploying'
            }
        }
   }

}
