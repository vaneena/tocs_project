
pipeline {
    agent any

    stages {
        stage('Deploy HTML, CSS, and JS') {
            steps {
                sshagent(['Apache']) {
                    script {
                        echo 'Deploying web files to the remote Apache server...'

                        // Automatically add the server's SSH key to known_hosts
                        sh '''
                        mkdir -p ~/.ssh
                        chmod 700 ~/.ssh
                        ssh-keyscan -H 172.31.41.69 >> ~/.ssh/known_hosts
                        chmod 644 ~/.ssh/known_hosts
                        '''

                        // Copy the files to the remote server
                        sh '''
                        scp -o StrictHostKeyChecking=no index.html script.js style.css ubuntu@172.31.41.69:/var/www/html/
                        '''
                    }
                }
            }
        }

        stage('Verify Deployment') {
            steps {
                script {
                    echo 'Verifying deployment of web files on the remote server...'

                    // Check if index.html is accessible
                    sh '''
                    curl -s --fail http://172.31.41.69/index.html || echo "Error: index.html not accessible"
                    '''

                    // Optionally verify CSS and JS files (uncomment if needed)
                    // sh 'curl -s --fail http://172.31.41.69/style.css || echo "Error: style.css not accessible"'
                    // sh 'curl -s --fail http://172.31.41.69/script.js || echo "Error: script.js not accessible"'
                }
            }
        }
    }

    post {
        success {
            echo 'Deployment of web files completed successfully!'
        }
        failure {
            echo 'Deployment failed. Check the logs for more details.'
        }
    }
}
