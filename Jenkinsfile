pipeline {
	agent any
	triggers {
	//Query repository weekdays every four hours starting at minute 0
	        git changelog: false, credentialsId: 'c80a64a5-d56e-46d6-858b-ac8c16801ae4', url: 'https://github.com/alamscode/nginx-sample'
            pollSCM('* * * * *')
            }
	stages {
		stage ('build') {
		steps {	  
                sh 'echo "A one line step"'
            }
		}
		stage ('test: integration-&-quality') {
		steps {	  
                sh 'echo "A one line step"'
            }
		}
		stage ('test: functional') {
		steps {	  
                sh 'echo "A one line step"'
            }
		}
		stage ('test: load-&-security') {
		steps {	  
                sh 'echo "A one line step"'
            }
		}
		stage ('approval') {
		steps {	  
                sh 'echo "A one line step"'
            }
		}
		stage ('deploy:prod') {
		steps {	  
                sh 'echo "A one line step"'
            }
		}
	}
}
