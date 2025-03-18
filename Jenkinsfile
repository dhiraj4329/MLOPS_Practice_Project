pipeline{
    agent any

    environment {
        VENV_DIR = 'venv'
    }

    stages{
        stage('cloning Github repo to jenkins'){
            steps{
                script{
                    echo 'cloning Github repo to jenkins.............'
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github-token', url: 'https://github.com/dhiraj4329/MLOPS_Practice_Project.git']])
                }
            }
        }
        stage('setting up virtual environment and installing dependencies'){
            steps{
                script{
                    echo 'setting up virtual environment and installing dependencies.............'
                    sh '''
                    python -m venv ${VENV_DIR}
                    . ${VENV_DIR}/bin/activate
                    pip install --upgrade pip
                    pip install -e .
                    '''
                }
            }
        }
    }
} 