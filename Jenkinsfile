pipeline {
    agent { dockerfile true }
    stages {
	stage('Build&Run') {
	    failFast true
            parallel {
                stage('Build App1') {
                    steps {
                        sh 'docker-compose -f docker/docker-compose.yml up -d app1'
                    }
                }
                stage('Build App2') {
                    steps {
			sh 'docker-compose -f docker/docker-compose.yml up -d app2'
                    }
                }
            }
        }
	stage('Checking') {
	    failFast true
	    parallel {
	        stage('Check App1') {
		    steps {
                        script {
			    def response = httpRequest 'http://127.0.0.1:8888'
			    if (response.content == 'Hello World from app1!') {
				println("Content: "+response.content)
			    } else {
				error("App1 did not respond correctly.")
			    }
			}
		    }
	        }
	        stage('Check App2') {
		    steps {
			script {
			    def response = httpRequest 'http://127.0.0.1:8889'
			    if (response.content == 'Hello World from app2!') {
                                println("Content: "+response.content)
			    } else {
				error("App2 did not respond correctly.")
			    }
			}
		    }
	        }
	    }
	}

    }
}
