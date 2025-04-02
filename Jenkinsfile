pipeline {
    agent { label 'ubuntu-agent' } // Runs on Ubuntu 22.04 Jenkins agent
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/<your-username>/flask-app-cicd.git', branch: 'main'
            }
        }
        stage('Build and Package') {
            steps {
                sh 'tar -czf app.tar.gz -C app .'
                archiveArtifacts artifacts: 'app.tar.gz', fingerprint: true
            }
        }
        stage('Deploy') {
            steps {
                sh 'ansible-playbook -i ansible/inventory.yml ansible/deploy.yml'
            }
        }
    }
}
