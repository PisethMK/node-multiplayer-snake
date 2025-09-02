node ('app'){  
    def app
    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
       checkout scm
    }  
    
    stage('Build-and-Tag') {
    /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build("piseth168/snake")
    }
    stage('Post-to-dockerhub') {
    
     docker.withRegistry('https://registry.hub.docker.com', 'docker-crendential') {
            app.push("latest")
        			}
         }
    
    stage('Deploy to VM') {
        withEnv(["SSH_KEY=/home/appserver/.ssh/id_rsa"]) {
            sh """
                ssh -i $SSH_KEY -o StrictHostKeyChecking=no appserver@192.168.58.14 '
                    cd node-multiplayer-snake &&
                    docker-compose down &&
                    docker-compose up -d
                '
            """
        }
    }

}
