String cron_string = BRANCH_NAME == "dev" ? "* * * * *" : "@yearly"

pipeline {
	
	
	agent any
	options {
    		disableConcurrentBuilds()
		}
        triggers { pollSCM(cron_string) }
            
	stages {
		stage ('build') {
		steps {	  
                sh 'echo "Hello World!!"'
		sh 'sleep 10'
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
