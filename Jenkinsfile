pipeline {
    agent any

    environment {
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')   // yeh tumne Jenkins Credentials me save kiya tha
        SITE_ID = '9664e49a-5d63-4e18-8e82-4c51502ed9f6'     // Netlify Site API ID
    }

    stages {
        stage('Deploy to Netlify') {
            steps {
                sh '''
                    echo "🚀 Zipping site..."
                    zip site.zip index.html

                    echo "🚀 Deploying to Netlify..."
                    curl -H "Authorization: Bearer $NETLIFY_AUTH_TOKEN" \
                         -H "Content-Type: application/zip" \
                         --data-binary "@site.zip" \
                         https://api.netlify.com/api/v1/sites/$SITE_ID/deploys
                '''
            }
        }
    }
}
