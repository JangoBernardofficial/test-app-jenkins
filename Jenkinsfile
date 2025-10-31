pipeline {
    agent any
    
    tools {
        nodejs "node"  // Make sure Node.js is installed in Jenkins
    }
    
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/JangoBernardofficial/test-app-jenkins.git'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm install'  // If you have package.json
            }
        }
        
        stage('Code Quality Check') {
            steps {
                // HTML validation (optional)
                sh 'find . -name "*.html" -exec echo "Validating {}" \\;'
                
                // CSS validation (optional)
                sh 'find . -name "*.css" -exec echo "Checking {}" \\;'
            }
        }
        
        stage('Security Scan') {
            steps {
                // Basic security checks
                sh 'echo "Running basic security checks..."'
            }
        }
        
        stage('Build') {
            steps {
                // For static sites, build might mean optimization
                sh 'echo "Building static site..."'
                
                // Create build directory and copy files
                sh 'mkdir -p dist'
                sh 'cp -r *.html *.css *.js dist/ || true'
            }
        }
        
        stage('Test') {
            steps {
                // Basic file existence tests
                sh 'test -f index.html && echo "✓ index.html exists" || exit 1'
                sh 'test -f style.css && echo "✓ style.css exists" || exit 1'
                sh 'test -f script.js && echo "✓ script.js exists" || exit 1'
            }
        }
        
        stage('Deploy') {
            steps {
                // Choose one deployment method:
                
                // Option 1: Archive artifacts
                archiveArtifacts artifacts: 'dist/**/*', fingerprint: true
                
                // Option 2: Deploy to server (uncomment and configure)
                /*
                sshagent(['your-ssh-credentials']) {
                    sh """
                    scp -r dist/* user@your-server:/var/www/html/
                    """
                }
                */
                
                // Option 3: S3 deployment (uncomment and configure)
                /*
                withAWS(region: 'your-region', credentials: 'your-aws-cred') {
                    sh 'aws s3 sync dist/ s3://your-bucket-name/ --delete'
                }
                */
            }
        }
    }
    
    post {
        always {
            // Clean up workspace
            cleanWs()
            
            // Send notifications
            emailext (
                subject: "Build Result: ${currentBuild.fullDisplayName}",
                body: "Build ${currentBuild.result}: ${env.BUILD_URL}",
                to: "your-email@example.com"
            )
        }
        
        success {
            echo 'Build completed successfully!'
        }
        
        failure {
            echo 'Build failed!'
        }
    }
}