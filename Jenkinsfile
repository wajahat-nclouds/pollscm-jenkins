pipeline {
	
	agent any
	
	triggers {
	//Query repository weekdays every four hours starting at minute 0
            pollSCM('* * * * *')
            }
	stages {
		stage ('build') {
		steps {	  
                sh 'echo "A one line step one"'
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
