pipeline {
    agent any

    environment {
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
        SITE_ID = '9664e49a-5d63-4e18-8e82-4c51502ed9f6'
    }

    stages {
        stage('Checkout Repo') {
            steps {
                // Agar Git repo hai to checkout karo
                checkout scm
            }
        }

        stage('Prepare Deployment') {
            steps {
                bat '''
                    echo 🚀 Preparing site for deployment...

                    REM Delete old zip if exists
                    if exist site.zip del site.zip

                    REM Compress all HTML, CSS, JS files directly into root of zip
                    powershell Compress-Archive -Path *.html, *.css, *.js -DestinationPath site.zip -Force

                    echo ✅ Deployment package ready: site.zip
                '''
            }
        }

        stage('Deploy to Netlify') {
            steps {
                bat '''
                    echo 🚀 Deploying to Netlify...

                    curl -H "Authorization: Bearer %NETLIFY_AUTH_TOKEN%" ^
                         -H "Content-Type: application/zip" ^
                         --data-binary "@site.zip" ^
                         https://api.netlify.com/api/v1/sites/%SITE_ID%/deploys

                    echo ✅ Deployment completed.
                '''
            }
        }
    }
}
