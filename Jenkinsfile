pipeline{
	agent any

	enviornment {
		EC2_USER = 'ubuntu'
		EC2_HOST = '35.154.236.204'
		SSH_CREDENTIAL_ID = 'ec2-ssh-key'
	}
	
	stages{
		stage('build'){
			steps{
					echo "Start Building"
					sh 'npm install'
					sh 'npm run build'
					echo "Building Complete"
				}
			}

		stage('deploy') {
			steps{
				echo 'Starting Deployment to EC2...'

					sshagent(['ec2-ssh-key']) {
						ssh '''
							echo "Copying build artifacts to EC2"
							sudo cp -r dist/ /var/www/html
							echo "Restarting Web Server On EC2"
							echo "Starting Deployment On EC2"
						'''
						}
						echo "Deployment Complete"
					}
		}
	}
}