pipeline {
	
	agent any
	
	triggers {
	//Query repository weekdays every four hours starting at minute 0
            pollSCM('* * * * *')
            }
	stages {
		stage ('build') {
		steps {	  
                sh 'echo "Hello!"'
            }
		}
		stage ('test: integration-&-quality') {
		steps {	  
                sh 'echo "Stage 2 done"'
            }
		}
		stage ('test: functional') {
		steps {	  
                sh 'echo "Stage 3 done"'
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
