pipeline {
    agent { label 'ubuntu-agent' }
    environment {
        APP_NAME = 'flask_app'
        VERSION = "${BUILD_NUMBER}"
        ARTIFACT = "${APP_NAME}_v${VERSION}.tar.gz"
        ARTIFACT_DIR = 'artifacts'
    }
    stages {
        stage('Clone Code') {
            steps {
                git url: 'https://github.com/suoton/ansible-team-11.git', branch: 'main'
            }
        }
        stage('Install Dependencies') {
            steps {
                dir('app') {
                    sh '''
                        python3 -m venv venv
                        . venv/bin/activate
                        pip install -r requirements.txt
                    '''
                }
            }
        }
        stage('Package App') {
            steps {
                sh """
                    mkdir -p ${ARTIFACT_DIR}
                    tar -czf ${ARTIFACT_DIR}/${ARTIFACT} -C app .
                """
            }
        }
        stage('Archive Artifact') {
            steps {
                archiveArtifacts artifacts: "${ARTIFACT_DIR}/${ARTIFACT}", fingerprint: true
            }
        }
        stage('Deploy with Ansible') {
            steps {
                // Assuming Ansible is installed and configured on the agent
                sh '''
                    cd ansible
                    ansible-playbook -i inventory/hosts playbook.yml \
                        --extra-vars "artifact_name=${ARTIFACT}"
                '''
            }
        }
    }
    post {
        success {
            echo "Deployment complete! Visit your Flask app on port 5000."
        }
        failure {
            echo "Build failed. Check logs and fix errors."
        }
    }
}
