pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('SONARQUBE_TOKEN')
        GITHUB_TOKEN = credentials('GITHUB_TOKEN')
        REPO_NAME = 'ranamahmoud12/templatemo-static-site'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: "${env.BRANCH_NAME}",
                    url: "https://github.com/${REPO_NAME}.git",
                    credentialsId: 'GITHUB_TOKEN'
            }
        }

        stage('Build & Package') {
            steps {
                sh '''
                zip -r templatemo_website.zip *
                '''
            }
        }

        stage('Deploy to GitHub Pages') {
            steps {
                sh '''
                git config user.email "jenkins@ranamahmoud.com"
                git config user.name "Jenkins CI"
                git clone -b gh-pages https://ranamahmoud12:${GITHUB_TOKEN}@github.com/${REPO_NAME}.git gh-pages
                rm -rf gh-pages/*
                cp -r * gh-pages/
                cd gh-pages
                git add .
                git commit -m "Deploy from Jenkins branch ${BRANCH_NAME}"
                git push origin gh-pages
                '''
            }
        }
    }
}
