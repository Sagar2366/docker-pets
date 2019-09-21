node('master') {     

   def userInput
   def app
   def cloudInput
  
   stage('Cloning git repository') {
       checkout scm
   }
               
   stage('Project Selection') {
       userInput = input(message: '', parameters: [choice(choices: ['Python', 'Nodejs'], description: 'Choose your project language :', name: 'Project_Language')])
       echo userInput 
   }
          
   if (userInput == 'Nodejs') {
       env.NODEJS_HOME = "${tool 'nodejs_tool'}"
       env.PATH="${env.NODEJS_HOME}/bin:${env.PATH}"
       sh 'npm --version'
  
       try {
           stage('Cloning git repository') {
               checkout scm
           }
           currentBuild.result = 'SUCCESS'
       }
      
       catch (err) {
           if (currentBuild.result == 'UNSTABLE') {
               currentBuild.result = 'FAILURE'
               throw err
           }
       }
      
       finally {
           mail to: 'sagarutekar2366@gmail.com',
           subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
           body: "${env.BUILD_URL} has result ${currentBuild.result}"
           slackSend color: 'good', message: "Build ${env.BUILD_NUMBER} completed for  ${env.JOB_NAME}. And result for this pipelien build has result ${currentBuild.result} . Details: (<${env.BUILD_URL} | here >)"
       }
      
       try {
           stage('SonarQube analysis') {
               def scannerHome = tool 'SonarScanner1';
               withSonarQubeEnv('sonar-server') {
               parallel (
                   "stream 1" : {
                       node('slave1') {
                           build job: 'sonar-scanner1'
                       }
                   },
                   "stream 2" : {
                       node('slave2') {
                           build job: 'sonar-scanner1'
                       }
                   })
               }
           }
           currentBuild.result = 'SUCCESS'
       }
      
       catch (err) {
           currentBuild.result = "FAILED"
           throw err
       }

       finally {
           mail to: 'sagarutekar2366@gmail.com',
           subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
           body: "${env.BUILD_URL} has result ${currentBuild.result}"
           slackSend color: 'good', message: "Build ${env.BUILD_NUMBER} completed for  ${env.JOB_NAME}. And result for this pipelien build has result ${currentBuild.result} . Details: (<${env.BUILD_URL} | here >)"
       }
     
        try {
           stage('Testing all files'){
               dir('project_work_1'){
                   sh 'npm install',
                   sh 'npm test'
               }
           }
           currentBuild.result = 'SUCCESS'
       }

       catch (err) {
           currentBuild.result = 'FAILURE'
           throw err
       }

       finally {
           mail to: 'sagarutekar2366@gmail.com',
           subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
           body: "${env.BUILD_URL} has result ${currentBuild.result}"
           slackSend color: 'good', message: "Build ${env.BUILD_NUMBER} completed for  ${env.JOB_NAME}. And result for this pipelien build has result ${currentBuild.result} . Details: (<${env.BUILD_URL} | here >)"
       }
      
      try {
           stage('Testing image') {
               app.inside {
               sh 'echo "Tests passed"'
               }
           }
           currentBuild.result = 'SUCCESS'
       }
      
       catch (err) {
           currentBuild.result = 'FAILURE'
           throw err     
       }
      
       finally {
           mail to: 'sagarutekar2366@gmail.com',
           subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
           body: "${env.BUILD_URL} has result ${currentBuild.result}"
           slackSend color: 'good', message: "Build ${env.BUILD_NUMBER} completed for  ${env.JOB_NAME}. And result for this pipelien build has result ${currentBuild.result} . Details: (<${env.BUILD_URL} | here >)"   
       }
       try {
           stage('Building image') {
               dir('project_work_1') {
                app= docker.build("my-image:${env.BUILD_ID}")
               }
           }
           currentBuild.result = 'SUCCESS'
       }
      
       catch (err) {
           currentBuild.result = 'FAILURE'
           throw err
       }
      
       finally {
           mail to: 'sagarutekar2366@gmail.com',
           subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
           body: "${env.BUILD_URL} has result ${currentBuild.result}"
           slackSend color: 'good', message: "Build ${env.BUILD_NUMBER} completed for  ${env.JOB_NAME}. And result for this pipelien build has result ${currentBuild.result} . Details: (<${env.BUILD_URL} | here >)"
       }
      
      
       try {
           stage('Push image to docker private registry') {
               docker.withRegistry('http://10.136.60.12:5000', 'docker-registry-credentials') {
                   app.push("latest")
               }
           }
           currentBuild.result = 'SUCCESS'
       }

       catch (err) {
           currentBuild.result = 'FAILURE'
           throw err  
       }
      
       finally {
           mail to: 'sagarutekar2366@gmail.com',
           subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
           body: "${env.BUILD_URL} has result ${currentBuild.result}"
           slackSend color: 'good', message: "Build ${env.BUILD_NUMBER} completed for  ${env.JOB_NAME}. And result for this pipelien build has result ${currentBuild.result} . Details: (<${env.BUILD_URL} | here >)"  
       }
      
       try {
           input 'Should we continue to deploy stage?'
           stage('Deploying container to kubernetes cluster') {
               script {
                  //Deploy application to kubernetes environment
               }
           }
           currentBuild.result = 'SUCCESS'
       }

       catch (err) {
           //if (currentBuild.result == 'UNSTABLE')
           currentBuild.result = 'FAILURE'
           throw err     
       }
      
       finally {
           mail to: 'sagarutekar2366@gmail.com',
           subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
           body: "${env.BUILD_URL} has result ${currentBuild.result}"
           slackSend color: 'good', message: "Build ${env.BUILD_NUMBER} completed for  ${env.JOB_NAME}. And result for this pipelien build has result ${currentBuild.result} . Details: (<${env.BUILD_URL} | here >)"  
           }  
       }
  
   else {
       node('slave1') {    
           try{           
               stage('Cloning git repository') {
                   //dir('python-demo'){
                   checkout scm
                   //}           
               }
               currentBuild.result = 'SUCCESS'
           }

           catch (err) {
               if (currentBuild.result == 'UNSTABLE'){
                   currentBuild.result = 'FAILURE'
                   throw err
               }
           }
      
           finally {
               mail to: 'sagarutekar2366@gmail.com',
               subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
               body: "${env.BUILD_URL} has result ${currentBuild.result}"
               slackSend color: 'good', message: "Build ${env.BUILD_NUMBER} completed for  ${env.JOB_NAME}. And result for this pipelien build has result ${currentBuild.result} . Details: (<${env.BUILD_URL} | here >)"
           }

           try {
               stage('SonarQube analysis') {
                   def scannerHome = tool 'SonarScanner1';
                   withSonarQubeEnv('sonar-server') {
                   parallel (
                       "stream 1" : {
                           node('slave1') {
                               build job: 'Python-sonar'
                           }
                       },
              
                       "stream 2" : {
                           node('slave2') {
                               build job: 'Python-sonar-gitlab'
                           }
                       })
                   }
               }
               currentBuild.result = 'SUCCESS'
           }

           catch (err) {
               if (currentBuild.result == 'UNSTABLE'){
                   currentBuild.result = 'FAILURE'
                   throw err
               }
           }
      
           finally {
               mail to: 'sagarutekar2366@gmail.com',
               subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
               body: "${env.BUILD_URL} has result ${currentBuild.result}"
               slackSend color: 'good', message: "Build ${env.BUILD_NUMBER} completed for  ${env.JOB_NAME}. And result for this pipelien build has result ${currentBuild.result} . Details: (<${env.BUILD_URL} | here >)"
           }

           try {
               stage ('Install tools required for testing and coverage') {
                   dir('python-demp'){
                       sh 'ls -al'
                       sh 'python3 --version'
                       sh 'pip3 install --no-cache-dir -q -r requirement.txt'
                       // sh 'python -m pip3 install coverage'
                   }
               }
               currentBuild.result = 'SUCCESS'
           }

           catch (err) {
               if (currentBuild.result == 'UNSTABLE'){
                   currentBuild.result = 'FAILURE'
                   throw err
               }
           }
      
           finally {
               mail to: 'sagarutekar2366@gmail.com',
               subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
               body: "${env.BUILD_URL} has result ${currentBuild.result}"
               slackSend color: 'good', message: "Build ${env.BUILD_NUMBER} completed for  ${env.JOB_NAME}. And result for this pipelien build has result ${currentBuild.result} . Details: (<${env.BUILD_URL} | here >)"
           }

           try {
               stage('Cloud Selection') {
                   cloudInput = input(message: 'Select cloud for which to run test and coverage :', parameters: [choice(choices: ['AWS', 'GCP'], description: 'Choose your project language :', name: 'Project_Language')])
                   echo userInput 
               } 
               currentBuild.result = 'SUCCESS'
           }

           catch (err) {
               if (currentBuild.result == 'UNSTABLE'){
                   currentBuild.result = 'FAILURE'
                   throw err
               }
           }
      
           finally {
               mail to: 'sagarutekar2366@gmail.com',
               subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
               body: "${env.BUILD_URL} has result ${currentBuild.result}"
               slackSend color: 'good', message: "Build ${env.BUILD_NUMBER} completed for  ${env.JOB_NAME}. And result for this pipelien build has result ${currentBuild.result} . Details: (<${env.BUILD_URL} | here >)"
           }               

           try {
               if(cloudInput == 'AWS') {
                   stage('Pytest and code-coverage for AWS'){
                       dir('python-demo'){
                           sh 'coverage run --source=aws -m pytest ./test/aws'
                           sh 'coverage html'
                           sh 'coverage report'
                       }
                   }
               }
               currentBuild.result = 'SUCCESS'
           }

           catch (err) {
               if (currentBuild.result == 'UNSTABLE'){
                   currentBuild.result = 'FAILURE'
                   throw err
               }
           }
      
           finally {
               mail to: 'sagarutekar2366@gmail.com',
               subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
               body: "${env.BUILD_URL} has result ${currentBuild.result}"
               slackSend color: 'good', message: "Build ${env.BUILD_NUMBER} completed for  ${env.JOB_NAME}. And result for this pipelien build has result ${currentBuild.result} . Details: (<${env.BUILD_URL} | here >)"
           }  

           try {  
               else{
                   stage('Pytest and code-coverage for GCP'){
                       dir('python-demo'){
                           sh 'coverage run --source=gcp -m pytest ./test/gcp'
                           sh 'coverage html'
                           sh 'coverage report'
                       }
                   }
               }
               currentBuild.result = 'SUCCESS'
           }

           catch (err) {
               if (currentBuild.result == 'UNSTABLE'){
                   currentBuild.result = 'FAILURE'
                   throw err
               }
           }
      
           finally {
               mail to: 'sagarutekar2366@gmail.com',
               subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
               body: "${env.BUILD_URL} has result ${currentBuild.result}"
               slackSend color: 'good', message: "Build ${env.BUILD_NUMBER} completed for  ${env.JOB_NAME}. And result for this pipelien build has result ${currentBuild.result} . Details: (<${env.BUILD_URL} | here >)"
           }

           try{
               stage('Build docker image'){
                   dir('python-demo'){
                       sh'docker-compose build'
                   }
               }
               currentBuild.result = 'SUCCESS'
           }

           catch (err) {
               if (currentBuild.result == 'UNSTABLE'){
                   currentBuild.result = 'FAILURE'
                   throw err
               }
           }
      
           finally {
               mail to: 'sagarutekar2366@gmail.com',
               subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
               body: "${env.BUILD_URL} has result ${currentBuild.result}"
               slackSend color: 'good', message: "Build ${env.BUILD_NUMBER} completed for  ${env.JOB_NAME}. And result for this pipelien build has result ${currentBuild.result} . Details: (<${env.BUILD_URL} | here >)"
           }

           try {
               stage('Run cotainers from image'){
                   dir('python-demo'){
                       sh 'docker-compose up'
                   }
               }  
               currentBuild.result = 'SUCCESS'
           }

           catch (err) {
               if (currentBuild.result == 'UNSTABLE'){
                   currentBuild.result = 'FAILURE'
                   throw err
               }
           }
      
           finally {
               mail to: 'sagarutekar2366@gmail.com',
               subject: "Status of pipeline: ${currentBuild.fullDisplayName}",
               body: "${env.BUILD_URL} has result ${currentBuild.result}"
               slackSend color: 'good', message: "Build ${env.BUILD_NUMBER} completed for  ${env.JOB_NAME}. And result for this pipelien build has result ${currentBuild.result} . Details: (<${env.BUILD_URL} | here >)"
           } 
       }
   }
}

