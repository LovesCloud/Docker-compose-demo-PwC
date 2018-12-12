node{ 
   stage('Checkout'){        
         def gitclone= "git clone 'https://github.com/LovesCloud/Docker-compose-demo.git'"         
         sshagent(['k8_master']) {
              sh "ssh -o StrictHostKeyChecking=no admin@54.236.63.5 ${gitclone}"            
        }
  }
   stage('Docker-compose to K8s conversion'){        
            def kompose= "kompose convert -f /home/admin/Docker-compose-demo/kompose.yaml"         
            sshagent(['k8_master']) {
              sh "ssh -o StrictHostKeyChecking=no admin@54.236.63.5 ${kompose}"
            }
   }
  stage('frontend-deployment'){
            def kubectl= "kubectl create -f /home/admin/Docker-compose-demo/frontend-deployment.yaml"         
            sshagent(['k8_master']) {
              sh "ssh -o StrictHostKeyChecking=no admin@54.236.63.5 ${ kubectl}"
            }
  }   
  stage('backend-deployment'){        
            def kubectl= "kubectl create -f /home/admin/Docker-compose-demo/backend-deployment.yaml"         
            sshagent(['k8_master']) {
              sh "ssh -o StrictHostKeyChecking=no admin@54.236.63.5 ${kubectl}"
            }
  }  
  stage('redis-deployment'){        
            def kubectl= "kubectl create -f /home/admin/Docker-compose-demo/redis-deployment.yaml"         
            sshagent(['k8_master']) {
              sh "ssh -o StrictHostKeyChecking=no admin@54.236.63.5 ${kubectl}"
            }
  }
  stage('frontend-service'){        
            def kubectl= "kubectl create -f /home/admin/Docker-compose-demo/frontend-service.yaml"         
            sshagent(['k8_master']) {
              sh "ssh -o StrictHostKeyChecking=no admin@54.236.63.5 ${kubectl}"         
            }
  }
  stage('backend-service'){        
            def kubectl= "kubectl create -f /home/admin/Docker-compose-demo/backend-service.yaml"         
            sshagent(['k8_master']) {
              sh "ssh -o StrictHostKeyChecking=no admin@54.236.63.5 ${kubectl}"         
            }
  }
  stage('redis-service'){        
            def kubectl= "kubectl create -f /home/admin/Docker-compose-demo/redis-service.yaml"         
            sshagent(['k8_master']) {
              sh "ssh -o StrictHostKeyChecking=no admin@54.236.63.5 ${kubectl}"         
            }
  }
   stage('Exposing Frontend'){        
            def kubectl= "kubectl expose deployment frontend --type=LoadBalancer --name=frontend"      
            sshagent(['k8_master']) {
              sh "ssh -o StrictHostKeyChecking=no admin@54.236.63.5 ${kubectl}"              
            }
  }
}
