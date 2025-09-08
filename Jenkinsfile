pipeline {
    agent any

    environment {
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
        SITE_ID = '9664e49a-5d63-4e18-8e82-4c51502ed9f6'
    }

    stages {
        stage('Checkout Repo') {
            steps {
                checkout scm
            }
        }

        stage('Prepare Deployment') {
            steps {
                bat '''
                    echo 🚀 Preparing deployment zip...

                    REM Delete old zip if exists
                    if exist site.zip del site.zip

                    REM Compress everything in repo root into site.zip (preserves folder structure)
                    powershell Compress-Archive -Path * -DestinationPath site.zip -Force

                    echo ✅ Deployment package ready
                '''
            }
        }

        stage('Deploy to Netlify') {
            steps {
                bat '''
                    echo 🚀 Deploying site.zip to Netlify...

                    REM Use -k flag to bypass Windows curl SSL issues
                    curl -k -H "Authorization: Bearer %NETLIFY_AUTH_TOKEN%" ^
                         -H "Content-Type: application/zip" ^
                         --data-binary "@site.zip" ^
                         https://api.netlify.com/api/v1/sites/%SITE_ID%/deploys

                    echo ✅ Deployment completed.
                '''
            }
        }
    }

    post {
        success {
            echo " Deployment successful!"
        }
        failure {
            echo " Deployment failed. Check Jenkins logs."
        }
    }
}
