pipeline{
	agent any

	enviornment {
		DEPLOY_DIR = '/var/www/html'
		SERVICE_NAME = 'ngnix'
	}
	
	stages{
		stage('Build'){
			steps{
					echo "Installing Dependencies"
					sh 'npm install'

					echo "Building The Application..."
					sh 'npm run build'
					echo "Build Complete"
				}
			}

		stage('Deploy') {
			steps{
				echo 'Deploying Build To Local Web Server...'
					ssh '''
						echo "Copying build to $DEPLOY_DIR"
						sudo rm -rf ${DEPLOY_DIR}/*
						sudo cp -r dist/* ${DEPLOY_DIR}/

						echo "Restarting $SERVICE_NAME"
						sudo systemctl restart ${SERVICE_NAME}
					'''

					echo 'Deployment Completed'
					}
				}
		}
	}