pipeline { 
    agent any 
    stages { 
        stage('Build') { 
            steps { 
                sh 'echo "Hello World"' 
                sh ''' 
                    echo "Multiline shell steps work too" 
                    ls -lah 
                ''' 
            } 
        }       
        stage('Upload to AWS') { 
            steps { 
                withAWS(region: 'us-east-1', credentials: 'jenkinsaws') { 
                    sh 'echo "Uploading content with AWS creds"' 
                    
                    // Ensure file exists before upload
                    sh 'if [ ! -f index.html ]; then echo "index.html not found!" && exit 1; fi'
                    
                    // Upload to S3
                    s3Upload(
                        pathStyleAccessEnabled: true, 
                        payloadSigningEnabled: true, 
                        workingDir: '.', // Upload from current workspace
                        includePathPattern: '**', // Upload all files and subdirectories
                        bucket: 'group2-jenkins-s3'
                    )   
                } 
            } 
        } 
    } 
}
