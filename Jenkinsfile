String cron_string = BRANCH_NAME == "master" ? "@hourly" : ""

pipeline {
	
	agent any
        triggers { cron(cron_string) }
            
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
                sh 'echo "Stage 4 done"'
            }
		}
		stage ('approval') {
		steps {	  
                sh 'echo "Stage 5 done"'
            }
		}
		stage ('deploy:prod') {
		steps {	  
                sh 'echo "Stage 6 done!!"'
            }
		}
	}
}
