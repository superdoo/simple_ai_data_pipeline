pipeline {
    agent any
    stages {
        stage('Checkout Code') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'githubuseraccesstoken', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                    sh 'rm -rf simple_ai_data_pipeline'
                    sh 'git clone https://$GIT_USER:$GIT_PASS@github.com/superdoo/simple_ai_data_pipeline.git'
                }
            }
        }


   stage('Train Model') {
    steps {
        sh '''
            # Create venv if it doesn't exist
if [ ! -d "venv" ]; then
    python3 -m venv venv
fi

# Upgrade pip using the venv pip
./venv/bin/pip install --upgrade pip

# Install dependencies using the venv pip
./venv/bin/pip install -r requirements.txt

# Run training script using the venv python
./venv/bin/python train_model.py
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
