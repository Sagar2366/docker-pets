node {     

           stage('Cloning git repository') {
               checkout scm
           }

           stage('Build Application and installd dependencies') {
               sh "pip install flask==0.10.1 python-consul"
               sh "python app.py & python admin.py"
           }
           
           stage('Build docker image') {
               sh "docker build -t docker-pets ."
           }
           
           stage('Run docker container') {
               sh "docker run -it -p 5003:5000 docker-pets"
           }
           
        }
