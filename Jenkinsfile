import groovy.transform.Field

podTemplate(label: 'fgt', containers: [
	containerTemplate(name: 'fgt-helm', image: 'alpine/helm', command: 'cat', ttyEnabled: true)],
	volumes: [hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')]
	   ){
	node('fgt'){
		properties([
			parameters([
				string(
					defaultValue: 'latest',
					description: '',
					name: 'fe_tag',
					trim: true),
				string(
					defaultValue: 'latest',
					description: '',
					name: 'be_budget_tag',
					trim: true),
			])
		])
		
		stage("Helm install"){
			git 'https://akhilbommu-zemoso.github.io/fgt/'
			container('fgt-helm'){
				withCredentials([file(credentialsId: 'master2-rishmita', variable: 'config')]){
					sh """
					export KUBECONFIG = \${config}
					helm upgrade --install fgt fgt/fgt-chart -n fgt --set fetag=${fe_tag} --set betag=${be_budget_tag}
					"""
				}
			}
		}	
	}
}


// pipeline{
// 	agent{		
// 		kubernetes {
// yaml '''
// apiVersion: v1
// kind: Pod
// spec:
//   containers:
//   - name: fgt-helm
//     image: alpine/helm
//     command:
//     - cat
//     tty: true
// '''
//     		}
// 	}
// 	environment {
// 		MY_KUBECONFIG = credentials('master2-rishmita')
// 	}
// 	stages{
// 		stage("add Repo") {
// 			steps {
// 		    		container('fgt-helm'){
// 		       			sh "helm repo add fgt https://akhilbommu-zemoso.github.io/fgt/ "
// 		    		}
// 			}
// 		}
                    
//                 stage("Build") {
//                         steps {
//                             	container('fgt-helm'){
//                                		sh """
// 						export KUBECONFIG=\${MY_KUBECONFIG}
// 						helm repo update 
// 						helm upgrade --install fgt fgt/fgt-chart -n fgt                              
// 					"""
//                             	}
//                         }
// 		}
              
// 	}
// }

