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
        stage('Building and Pushing Docker Image to GCR'){
            steps{
                withCredentials([file(credentialsId: 'gcp-key' , variable : 'GOOGLE_APPLICATION_CREDENTIALS')]){
                    script{
                        echo 'Building and Pushing Docker Image to GCR.............'
                        sh '''
                        export PATH=$PATH:${GCLOUD_PATH}


                        gcloud auth activate-service-account --key-file=${GOOGLE_APPLICATION_CREDENTIALS}

                        gcloud config set project ${GCP_PROJECT}

                        gcloud auth configure-docker --quiet

                        docker build -t gcr.io/${GCP_PROJECT}/ml-project:latest .

                        docker push gcr.io/${GCP_PROJECT}/ml-project:latest 

                        '''
                    }
                }
            }
        }
        
    }
} 