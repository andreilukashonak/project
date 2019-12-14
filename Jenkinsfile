pipeline {
    agent { 
        label 'master'
    }
    environment {
        registry = 'dockerandrei/sa_project'
        registryCredential = 'docker-hub-credentials'
    }
    parameters {
        string(name: 'repository_url', defaultValue: 'git@github.com:andreilukashonak/project.git', description: 'Github repository url')
        booleanParam(name: 'build_and_run_docker', defaultValue: true, description: 'Deploy and run docker')
    	booleanParam(name: 'remove', defaultValue: false, description: 'Remove Dokuwiki')
  }
  stages {
     stage ('Build and run docker for Dokuwiki') {
	    when {
	     expression {params.build_and_run_docker == true}
	    }  
	      stages {
          stage('Clone Git') {
            steps {
              git url: "${params.repository_url}",
              credentialsId: '66f6eb3b-3cd3-492c-997b-6579c2ab1049'  ##########
            }
          }
          stage('Build image') {
            steps {
              script {
                dockerImage = docker.build registry + ":$BUILD_NUMBER"
              }
            }
          }	          
          stage('Push image') {
            steps {
              script {
                docker.withRegistry( '', registryCredential ) {
                  dockerImage.push()
                }
              }
            }
          }
          stage('Run docker image'){
            steps {
              sh "echo $registry:$BUILD_NUMBER"
              sh "docker run -d  -p 4000:80 --name dokuwiki $registry:$BUILD_NUMBER"
            }
          }  
	    }
     }
     stage('Remove Dokuwiki') {
	    when {
	     expression {params.remove == true}
	    }
        stages {
		  stage('Stop and delete docker container') {
		    steps {
                sh "docker container stop dokuwiki"
			    sh "docker container prune -f"
	        }
		  }
          stage('Remove docker image') {
            steps{
              sh "docker system prune -af"
            }
          }
        }
     }		
   }
   post {
    success {
      slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
      }
    failure {
      slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
      }
    }
}
