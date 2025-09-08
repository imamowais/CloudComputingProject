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

        stage('Deploy Single HTML to Netlify') {
            steps {
                bat '''
                    echo 🚀 Deploying index.html directly...

                    curl -H "Authorization: Bearer %NETLIFY_AUTH_TOKEN%" ^
                         -H "Content-Type: text/html" ^
                         --data-binary "@index.html" ^
                         https://api.netlify.com/api/v1/sites/%SITE_ID%/deploys

                    echo ✅ Deployment completed.
                '''
            }
        }
    }
}
