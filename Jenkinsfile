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
       # Remove any old venv completely
rm -rf venv

# Recreate the virtual environment
python3 -m venv venv

# FIX PERMISSIONS (important!)
chmod +x venv/bin/*

# Upgrade pip
venv/bin/python -m pip install --upgrade pip

# Install requirements
venv/bin/python -m pip install -r requirements.txt

# Run training
venv/bin/python train_model.py
 
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
