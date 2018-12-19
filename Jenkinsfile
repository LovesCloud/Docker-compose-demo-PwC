node{ 
   stage('Checkout'){ 
        def gitclone= "git clone 'https://github.com/LovesCloud/Docker-compose-demo.git'"  
        def remove= "rm -rf Docker-compose-demo/"
        def ip="`curl http://169.254.169.254/latest/meta-data/public-ipv4`"
        sshagent(['k8_master']) {
              sh "ssh -o StrictHostKeyChecking=no devops@$ip ${remove}"
              sh "ssh -o StrictHostKeyChecking=no devops@$ip ${gitclone}"            
        }
   }
   
   stage('Build'){    
            def ip="`curl http://169.254.169.254/latest/meta-data/public-ipv4`"
            def kompose= "kompose convert -f /home/devops/Docker-compose-demo/kompose.yaml"
            def rmyaml= "rm -rf *.yaml"
            sshagent(['k8_master']) {
              sh "ssh -o StrictHostKeyChecking=no devops@$ip ${rmyaml}"
              sh "ssh -o StrictHostKeyChecking=no devops@$ip ${kompose}"
            }
   }
  stage('Backend-Deployment'){
            def ip="`curl http://169.254.169.254/latest/meta-data/public-ipv4`"
            def kubectl_redis_frontend= "sudo kubectl create -f /home/devops/backend-deployment.yaml"   
            def kubectl_redis_service= "sudo kubectl create -f /home/devops/backend-service.yaml"
            sshagent(['k8_master']) {
              sh "ssh -o StrictHostKeyChecking=no devops@$ip ${kubectl_redis_frontend}"
              sh "ssh -o StrictHostKeyChecking=no devops@$ip ${kubectl_redis_service}"
            }
  }
  }
