node{
  
   stage('Checkout'){        
        def gitclone= "git clone 'https://github.com/LovesCloud/Docker-compose-demo.git'"         
         sshagent(['k8_master']) {
              sh "ssh -o StrictHostKeyChecking=no admin@54.236.63.5 ${gitclone}"
             
         }
}

   stage('Deploy'){        
            def kompose= "kompose up -f /home/admin/Docker-compose-demo/kompose.yaml"         
            sshagent(['k8_master']) {
              sh "ssh -o StrictHostKeyChecking=no admin@54.236.63.5 ${kompose}"              
}

   stage('Exposing Frontend'){        
            def kubectl= "kubectl expose deployment frontend --type=LoadBalancer --name=frontend1"      
            sshagent(['k8_master']) {
              sh "ssh -o StrictHostKeyChecking=no admin@54.236.63.5 ${kubectl}"              
}

}
       
   }
}
