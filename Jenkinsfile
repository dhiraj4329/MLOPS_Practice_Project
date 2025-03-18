pipeline{
    agent any

    environment {
        VENV_DIR = 'venv'
        GCP_PROJECT = "eternal-psyche-453105-m5"
        GCLOUD_PATH = "/var/jenkins_home/google-cloud-sdk/bin"
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
        stage('Deploy to Google Cloud Run'){
            steps{
                withCredentials([file(credentialsId: 'gcp-key' , variable : 'GOOGLE_APPLICATION_CREDENTIALS')]){
                    script{
                        echo 'Deploy to Google Cloud Run.............'
                        sh '''
                        export PATH=$PATH:${GCLOUD_PATH}


                        gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}

                        gcloud config set project ${GCP_PROJECT}

                        gcloud run deploy ml-project \
                            --image=gcr.io/${GCP_PROJECT}/ml-project:latest \
                            --platform=managed \
                            --region=us-central1 \
                            --allow-unauthenticated
                            
                        '''
                    }
                }
            }
        }
        
    }
} 