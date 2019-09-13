import groovy.json.JsonOutput

def isStartedByTimer() {
	def buildCauses = currentBuild.getBuildCauses()


	boolean isStartedByTimer = false
	for (buildCause in buildCauses) {
		if ("${buildCause}".contains("hudson.triggers.SCMTrigger")) {
			isStartedByTimer = true
		}
	}
	return isStartedByTimer
}


String cron_string = BRANCH_NAME == "dev" ? "* * * * *" : ""
def scm = "${isStartedByTimer()}"


pipeline {
	agent any
	parameters {
		string(name: 'GIT_REV', defaultValue: '', description: 'The git commit you want to build')
		string(name: 'EKS_CLUSTER', defaultValue: 'qa_nclouds', description: 'The name of the eks cluster')
		string(name: 'AWS_REGION', defaultValue: 'us-west-2')
		choice(name: 'OPTION', choices: ['test', 'build', 'deploy', 're-deploy'])
	}


	options {
		disableConcurrentBuilds()
	}
	triggers {
		pollSCM(cron_string)
	}

	stages {

		stage('Checkout') {
			when{
				not {
					expression {
						params.OPTION == "re-deploy"
					}
				}
			}
			
			steps {
				script {
					echo "Hello Wordld"
					echo "${scm}"
				}
			}
		}

		stage('build') {
			when {
				allOf {
					not {
						expression {
							params.OPTION == "re-deploy"
						}
					}
					expression {
						params.GIT_REV == ""
					}
				}
			}
			steps {
				sh 'echo "Stage build done"'
			}
		}


		stage('test') {
			when {
				allOf {
					not {
						expression {
							params.OPTION == "re-deploy"
						}
					}
				
					anyOf {
						expression {
							"${scm}" == "true"
						}
						not {
							allOf {
								expression {
									params.GIT_REV != ""
								}
								expression {
									params.OPTION == "deploy"
								}
							}
						}


					}
				}

			}
			steps {
				sh 'echo "Stage test done"'
			}
		}


		stage('push') {
			when {
				allOf {
					not {
						expression {
							params.OPTION == "re-deploy"
						}
					}
				
					anyOf {
						expression {
							"${scm}" == "true"
						}

						allOf {
							expression {
								params.GIT_REV == ""
							}
							anyOf {
								expression {
									params.OPTION == "deploy"
								}
								expression {
									params.OPTION == "build"
								}
							}
						}

					}
				}
			}

			steps {
				sh 'echo "Stage push done"'
			}
		}


		stage('deploy') {
			when {
				allOf {
					not {
						expression {
							params.OPTION == "re-deploy"
						}
					}
				
					anyOf {
						expression {
							"${scm}" == "true"
						}
						expression {
							params.OPTION == "deploy"
						}
					}
				}


			}
			steps {
				sh 'echo "Stage deploy done"'
				// error('failed')
			}

			post {
				success {
					echo "post deploy: success"
				}
			}
			
		}

		stage('re-deploy'){
			when {
				expression {
					params.OPTION == "re-deploy"
				}
			}
			steps{
				script {
					def str = '{"spec":{"template":{"metadata":{"labels":{"date":"mydate"}}}}}'
					def parser = new JsonSlurper()
					def json = parser.parseText(str)
					//def json = JsonOutput.toJson(data)
					//if you need pretty print (multiline) json
					json = JsonOutput.prettyPrint(json)

					//put string into the file:
					writeFile(file:'message2.json', text: json)
					sh 'cat message2.json'
					sh 'echo "Stage re-deploy done"'

				}
				
			}
		}

		
	}
}