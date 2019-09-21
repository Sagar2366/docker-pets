pipeline {

    agent none
    options {
        skipStagesAfterUnstable()
    }
    
    stages {
        stage('Build') {
            steps {
                echo "Building application started ...."
                sh git clone --recursive https://github.com/Sagar2366/docker-pets.git'
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
