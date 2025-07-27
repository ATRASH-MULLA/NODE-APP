pipeline{
    agent any

    environment{
        EC2_HOST = "13.233.102.138"             //ip of EC2 server
        SSH_CREDENTIAL_ID = "GLOBALCRED"        //global credential name
        REMOTE_USER = "ubuntu"
        REMOTE_PATH = "/home/ubuntu/app"
        WEB_ROOT = "/var/www/html"
        SERVER = "nginx"
    }

    stages{
        stage("Build"){
            steps{
                echo "Indtalling dependencies"
                sh "npm install"
                echo "Building the app"
                sh "npm run build"
                echo "Build complete"
            }
        }
        stage ("Deploy"){
            steps{
                echo "Starting deployment"
                sshagent(credential: [env.SSH_CREDENTIAL_ID]){
                     sh """
                        echo "Creating remote directory"
                        ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${EC2_HOST} 'mkdir -p ${REMOTE_PATH}'
                        
                        echo "Copying Distribution to remote path"
                        scp -o StrictHostKeyChecking=no -r dist/* ${REMOTE_USER}@${EC2_HOST}:${REMOTE_PATH}/

                        echo "Restarting nginx"
                        ssh -o StrictHostKeyChecking=no ${REMOTE_USER}@${EC2_HOST} '
                            sudo rm -rf ${WEB_ROOT}/*
                            sudo cp -r ${REMOTE_PATH}/* ${WEB_ROOT}/
                            sudo systemctl restart ${SERVER}
                        '
                    """
                }
            }
        }
    }
}