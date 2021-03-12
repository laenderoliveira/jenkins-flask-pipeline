pipeline {
	agent any
	
	options {
	  timestamps()
	  timeout(time: 10, unit: "MINUTES")
	}	
	
	environment {
	  IMAGE_NAME="simple-flask-app"
	  IMAGE_TAG="0.${BUILD_ID}"
	  CONTAINER_NAME="${IMAGE_NAME}:${IMAGE_TAG}"
	}
	stages{
	  stage("build"){
	    steps {
	      script {
	        image = docker.build("${CONTAINER_NAME}")
		image.inside("-v ${WORKSPACE}:/simplePythonApplication"){
		  sh "nosetests --with-xunit --cover-package=project test_users.py"
		}
	      }
	    }
	  }
	}
	post {

		success {
			echo "Pipeline finalizado com sucesso!!!!"
		}

		failure {
			echo " Pipeline erro"
		}

		cleanup {
			sh "docker image rm ${CONTAINER_IMAGE}"
		}

		always {
			junit 'nosetests.xml'
		}
	}
}