node (){  
    def app
    stage('Cloning Git') {
        /* Let's make sure we have the repository cloned to our workspace */
       checkout scm
    }  
    
    stage('Build-and-Tag') {
    /* This builds the actual image; synonymous to
         * docker build on the command line */
        app = docker.build("jmorales111/myprivaterepo")
    }

    
    stage('Test') {
        snykSecurity(
          snykInstallation: 'Snyk-latest',
          snykTokenId: 'interim_snyk',
          failOnIssues:false
        )

    }
    
    


    stage('Post-to-dockerhub') {
    
     docker.withRegistry('https://registry.hub.docker.com', 'interim-dockerhub') {
            app.push("latest")
        			}
    }
  
    
    stage('Pull-image-server') {
    
         sh "docker-compose down"
         sh "docker-compose up -d"	
      }
 
}
