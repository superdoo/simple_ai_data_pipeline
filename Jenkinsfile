pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'githubuseraccesstoken', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                    sh 'rm -rf advanced_ai_project'
                    sh 'git clone https://$GIT_USER:$GIT_PASS@github.com/superdoo/simple_ai_data_pipeline.git'
                }
            }
        }

   stage('Train Model') {
    steps {
        sh '''
            source ./venv/bin/activate
            python3 train_model.py
        '''
    }
}



        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ai-pipeline .'

            }
        }

        stage('Run Docker Compose') {
            steps {
                sh 'docker-compose up -d'
            }
        }
    }
}
