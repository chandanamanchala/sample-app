pipeline {
    agent any

    triggers {
        cron('H/15 * * * *') // Runs every 15 minutes
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/<your-username>/<repo-name>.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t my-react-app:latest .'
                }
            }
        }

        stage('Push to Registry') {
            steps {
                script {
                    sh 'docker tag my-react-app:latest registry.digitalocean.com/<your-registry>/my-react-app:latest'
                    sh 'docker push registry.digitalocean.com/<your-registry>/my-react-app:latest'
                }
            }
        }

        stage('Deploy to DigitalOcean') {
            steps {
                script {
                    // Assuming you use Docker on DO droplet
                    sh 'ssh root@<your-droplet-ip> "docker pull registry.digitalocean.com/<your-registry>/my-react-app:latest && docker stop react-app || true && docker run -d --name react-app -p 80:80 registry.digitalocean.com/<your-registry>/my-react-app:latest"'
                }
            }
        }
    }
}
