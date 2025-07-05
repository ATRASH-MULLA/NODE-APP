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
		}
	}