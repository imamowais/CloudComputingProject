pipeline {
    agent any

    environment {
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
        SITE_ID = '9664e49a-5d63-4e18-8e82-4c51502ed9f6'
    }

    stages {
        stage('Deploy to Netlify') {
            steps {
                bat '''
                    echo 🚀 Preparing site for deployment...

                    REM Delete old files if exist
                    if exist site.zip del site.zip
                    if exist site rmdir /s /q site

                    REM Create folder
                    mkdir site

                    REM Copy all project files (HTML, CSS, JS)
                    copy *.html site\\
                    copy *.css site\\
                    copy *.js site\\

                    REM Compress files directly (no nested folder)
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
