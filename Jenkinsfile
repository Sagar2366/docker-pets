pipeline {
    
    agent { docker { image 'python:3.7.2' } }
    
    stages {
        stage('Build') {
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
