def call(Map pipelineParams) {
String cron_string = BRANCH_NAME == "dev" ? "* * * * *" : ""

pipeline {
	
	
	parameters {
        string(name: 'GIT_REV', defaultValue: '', description: 'The git commit you want to build')
        string(name: 'EKS_CLUSTER', defaultValue: 'qa_nclouds', description: 'The name of the eks cluster')
        string(name: 'AWS_REGION', defaultValue: 'us-west-2')
        choice(name: 'OPTION', choices: ['test','build','deploy'])
    }
	
	
	agent any
	options {disableConcurrentBuilds()}
	triggers { pollSCM(cron_string) }
            
	stages {
		stage('Checkout') {
		    steps {sh 'echo "Hello!"'}
        }
    
		
	
		stage ('build') {
		when { expression { params.GIT_REV == "" }}
		steps {	  
                sh 'echo "Hello World!!"'
		        sh 'sleep 30'
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
                sh 'echo "Stage 2 done"'
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
                sh 'echo "Stage 3 done"'
            }
		}
	
	
		stage ('deploy') {
		when {
                expression { params.OPTION == "deploy" }
        	}	
		steps {	  
                sh 'echo "Stage 4 done"'
            }
		}
	}}}
