pipeline{
	agent any

	environment {
		EC2_HOST = '15.206.88.192'
		SSH_CREDENTIAL_ID = 'GLOBALCRED'
		REMOTE_USER = 'ubuntu'
		REMOTE_PATH = '/home/ubuntu/app'
		WEB_ROOT = '/var/www/html'
	}
	
	stages{
		stage('build'){
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
				echo 'Deploying to EC2 at ' + env.EC2_HOST
				sshagent (credentials: [env.SSH_CREDENTIAL_ID]){
				sh """
					echo "Creating remote directory..."
					ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${EC2_HOST} 'mkdir -p ${REMOTE_PATH}'

					echo "Copying build to remote EC2..."
					scp -o StrictHostKeyChecking=no -r dist/* ${REMOTE_USER}@${EC2_HOST}:${REMOTE_PATH}/

					echo "Moving files to web root and restarting nginx..."
					ssh ${REMOTE_USER}@${EC2_HOST} '
						sudo rm -rf ${WEB_ROOT}/*
						sudo cp -r ${REMOTE_PATH}/* ${WEB_ROOT}/
						sudo systemctl restart nginx
					'
				"""
			}

			echo 'Deployment Completed'
			}
		}
	}
}