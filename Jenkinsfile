pipeline{
		agent{
			
			kubernetes {
    
    yaml '''
        apiVersion: v1
        kind: Pod
        spec:
          containers:
          - name: fgt-helm
            image: alpine/helm
            command:
            - cat
            tty: true
        '''
    
    }
		}
    environment {
        MY_KUBECONFIG = credentials('config-file')
    }
		stages{
				
                stage("add Repo") {
                        steps {
                            container('fgt-helm'){
                               sh "helm repo add fgt https://pdhanrajnath.github.io/gc/ "
                            }
                            
                        }
                    }
                    
                stage("Build") {
                        steps {
                            container('fgt-helm'){
                               sh """
                                  export KUBECONFIG=\${MY_KUBECONFIG}
                                  helm repo update 
                                  helm install fgt fgt/ -n fgt                              
                                  """
                            }
                            
                        }
                    }
                    
                    
				
                
            }
