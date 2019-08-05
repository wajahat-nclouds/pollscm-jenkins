

//def isStartedByUser = currentBuild.rawBuild.getCause(hudson.model.Cause$UserIdCause)
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
		
		
		stage('find upstream job') {
            steps {
                script {
                    def causes = currentBuild.rawBuild.getCauses()
                    for(cause in causes) {
                        if (cause.class.toString().contains("UpstreamCause")) {
                            println "This job was caused by job " + cause.upstreamProject
                        } else {
                            println "Root cause : " + cause.toString()
                        }
                    }
                }      
            }
        }
		
		
		
		
		stage('Checkout') {
		    steps {
			    sh 'echo "Stage Checkout done"'
			    sh 'echo $isStartedByUser'
			    
			  }
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

