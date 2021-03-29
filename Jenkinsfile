pipeline {
	agent any
	stages {
		stage ('Buld Docker Images') {
			steps {
			sh """
			  cd "$WORKSPACE/
        docker build . \
  		    -t registry.internal.catapult-elearning.net:5000/cat_elasticsearch-dump \
	  			-t registry.internal.catapult-elearning.net:5000/cat_elasticsearch-dump:$BUILD_NUMBER \
		  		-t 391519401789.dkr.ecr.ap-southeast-2.amazonaws.com/cat_elasticsearch-dump \
			  	-t 391519401789.dkr.ecr.ap-southeast-2.amazonaws.com/cat_elasticsearch-dump:$BUILD_NUMBER
			"""
			}
		}
		stage ('Push Docker Images to Staging') {
			steps {
			sh """
        docker push registry.internal.catapult-elearning.net:5000/cat_elasticsearch-dump
  		  docker push registry.internal.catapult-elearning.net:5000/cat_elasticsearch-dump:$BUILD_NUMBER
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
          docker push 391519401789.dkr.ecr.ap-southeast-2.amazonaws.com/cat_elasticsearch-dump
			    docker push 391519401789.dkr.ecr.ap-southeast-2.amazonaws.com/cat_elasticsearch-dump:$BUILD_NUMBER
        """
			}
			}
		}
	}
}
