pipeline {
  agent any	
    stages {      
        stage('Build maven ') {
            steps { 
              sh 'pwd'      
              sh 'mvn  clean install package'
            }
        }
        
        stage('Copy Artifact') {
           steps { 
             sh 'pwd'
		     sh 'cp -r target/*.jar docker'
           }
        }
         
        stage('Build docker image') {
           steps {
               script {         
                 def customImage = docker.build('muzammilsayyed2606/petclinic', "./docker")
                 docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
                 customImage.push('latest')
                 }                     
           }
        }
	  }

      stage('Build on k8 ') {
            steps {
	      withKubeConfig([credentialsId: 'kubeconfig']) {
              sh 'pwd'
              sh 'cp -R helm/* .'
		      sh 'ls -ltr'
              sh 'pwd'
              sh '/usr/local/bin/helm upgrade --install petclinic-app petclinic  --set image.repository=muzammilsayyed2606/petclinic --set image.tag=latest'
              			
            }           
        }
      }
    }	    
}
