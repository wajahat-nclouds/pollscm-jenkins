//def cause = currentBuild.getBuildCauses('hudson.triggers.TimerTrigger$TimerTriggerCause')

// def isStartedByUser = currentBuild.rawBuild.getCause(hudson.model.Cause$UserIdCause)

def isStartedByTimer() {
    def buildCauses = currentBuild.rawBuild.getCauses()
    echo buildCauses

    boolean isStartedByTimer = false
    for (buildCause in buildCauses) {
        if ("${buildCause}".contains("hudson.triggers.TimerTrigger\$TimerTriggerCause")) {
            isStartedByTimer = true
        }
    }

    echo isStartedByTimer
    return isStartedByTimer
}

String cron_string = BRANCH_NAME == "dev" ? "* * * * *" : ""



pipeline {
	agent any
	parameters {
        string(name: 'GIT_REV', defaultValue: '', description: 'The git commit you want to build')
        string(name: 'EKS_CLUSTER', defaultValue: 'qa_nclouds', description: 'The name of the eks cluster')
        string(name: 'AWS_REGION', defaultValue: 'us-west-2')
        choice(name: 'OPTION', choices: ['test','build','deploy'])
    }
	
	
	
	options {disableConcurrentBuilds()}
	triggers { pollSCM(cron_string) }
            
	stages {

		stage('Checkout') {
		    steps {
			    script {
				    echo "${currentBuild.getBuildCauses('hudson.model.Cause$UserIdCause')}"
				    echo "${isStartedByTimer()}"
			    }}
        }
    
		stage ('build') {
		when { expression { params.GIT_REV == "" }}
		steps {	  
                sh 'echo "Stage build done"'
            }
		}
	
	
		stage ('test') {
		when { not { allOf {
                		        expression { params.GIT_REV != "" }
                		        expression { params.OPTION == "deploy"}
                        }
                    }
            }
		steps {	  
                sh 'echo "Stage test done"'
            }
		}
	
	
		stage ('push') {
		when { allOf { expression { params.GIT_REV == ""}
                     anyOf{
                           expression { params.OPTION  == "deploy" }
                           expression { params.OPTION == "build" }}
            }
        }
			
		steps {	  
                sh 'echo "Stage push done"'
            }
		}
	
	
		stage ('deploy') {
		when {
                expression { params.OPTION == "deploy" }
        	}	
		steps {	  
                sh 'echo "Stage deploy done"'
            }
		}
	}
}

