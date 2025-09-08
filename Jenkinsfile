pipeline {
    agent any

    environment {
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')   // Jenkins credentials
        SITE_ID = '9664e49a-5d63-4e18-8e82-4c51502ed9f6'     // Netlify Site ID
    }

    stages {
        stage('Deploy to Netlify') {
            steps {
                bat '''
                    echo 🚀 Preparing site for deployment...

                    REM Make a folder for site files
                    mkdir site
                    copy index.html site\\index.html

                    REM Zip the folder
                    powershell Compress-Archive -Path site\\* -DestinationPath site.zip -Force

                    echo 🚀 Deploying to Netlify...
                    curl -H "Authorization: Bearer %NETLIFY_AUTH_TOKEN%" ^
                         -H "Content-Type: application/zip" ^
                         --data-binary "@site.zip" ^
                         https://api.netlify.com/api/v1/sites/%SITE_ID%/deploys
                '''
            }
        }
    }
}
