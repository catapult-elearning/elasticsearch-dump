pipeline {
	agent any
	stages {
		stage ('Buld Docker Images') {
			steps {
			sh """
			  cd "$WORKSPACE/
        docker build . \
  		    -t registry.internal.catapult-elearning.net:5000/cat_elasticsearch \
	  			-t registry.internal.catapult-elearning.net:5000/cat_elasticsearch:$BUILD_NUMBER \
		  		-t 391519401789.dkr.ecr.ap-southeast-2.amazonaws.com/cat_elasticsearch \
			  	-t 391519401789.dkr.ecr.ap-southeast-2.amazonaws.com/cat_elasticsearch:$BUILD_NUMBER
			"""
			}
		}
		stage ('Push Docker Images to Staging') {
			steps {
			sh """
        docker push registry.internal.catapult-elearning.net:5000/cat_elasticsearch
  		  docker push registry.internal.catapult-elearning.net:5000/cat_elasticsearch:$BUILD_NUMBER
      """
			}
		}
		stage ('Push Docker Images to Production') {
			environment{
			 AWS_REGION = 'ap-southeast-2'
			}
			steps {
			withAWS(credentials:'aws_jenkins_staging') {
				script {
					def login = ecrLogin()
					sh "$login"
				}
				sh """
          docker push 391519401789.dkr.ecr.ap-southeast-2.amazonaws.com/cat_elasticsearch
			    docker push 391519401789.dkr.ecr.ap-southeast-2.amazonaws.com/cat_elasticsearch:$BUILD_NUMBER
        """
			}
			}
		}
	}
}
