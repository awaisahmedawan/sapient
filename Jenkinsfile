pipeline {
    agent { dockerfile true }


    stages {
	stage('Build') {
	    failFast true
            if (env.BRANCH_NAME == 'integration'){
                stage('Build docker') {
                    steps {
                        sh 'docker-compose -f docker/docker-compose.yml up -d app1'
                    }
                }
              }
            else if (env.BRANCH_NAME.startsWith('PR')){
              stage('Check Docker App') {
            steps {
                sh 'docker-compose -f docker/docker-compose.yml up -d app1'
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

            }
            else if(env.BRANCH_NAME == 'master'){
              stage('Deploy to Kubernetes') {
                  // deplloy to kubernetes
              }

            }


        }

    }
}
