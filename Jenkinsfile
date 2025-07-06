pipeline{
	agent any
		stages{
			stage('build'){
					steps{
						echo "Start Building"
						sh 'npm install'
						sh 'npm run build'
						echo "Building Complete"
				}
			}
			stage('build'){
				steps{
					echo "Starting Deployment on EC2"
					sh "scp -r -o strictCheckingOfKey=No ./dist* /home/ubuntunode-app/"
					sh 'npm start'
					echo "Deployment Done"
					}
		}
	}
