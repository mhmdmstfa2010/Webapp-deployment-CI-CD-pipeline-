pipeline {
    agent any

    environment {
        GIT_REPO = "https://github.com/mhmdmstfa2010/ODC-final-project.git"
        IMAGE_NAME = "final_project_image" // Docker image name
        DOCKER_TAG = "latest"
        DOCKER_HUB_REPO = "mohamed710/orange"
    }

    stages {
        stage('Clean Workspace') {
            steps {
                script {
                    // Clean up the workspace
                    deleteDir()
                }
            }
        }

        stage('Checkout Code') {
            steps {
                withCredentials([string(credentialsId: 'github', variable: 'GIT_TOKEN')]) {
                    sh '''
                        # Clone the repo and switch to the master branch
                        git clone https://$GIT_TOKEN@github.com/mhmdmstfa2010/ODC-final-project.git
                        cd ODC-final-project
                        git checkout master
                    '''
                }
            }
        }

        stage('Login to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        sh '''
                            # Log in to Docker Hub
                            echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin
                        '''
                    }
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh '''
                        # Navigate to the project directory and build the Docker image
                        cd ODC-final-project
                        docker build -t ${IMAGE_NAME}:${DOCKER_TAG} .
                    '''
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                    script {
                        sh '''
                            # Tag the Docker image with the Docker Hub repository name
                            docker tag ${IMAGE_NAME}:${DOCKER_TAG} ${DOCKER_HUB_REPO}:${DOCKER_TAG}
                            
                            # Push the Docker image to Docker Hub
                            docker push ${DOCKER_HUB_REPO}:${DOCKER_TAG}
                        '''
                    }
                }
            }
        }

        stage('List Docker Images') {
            steps {
                sh 'docker images'
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                script {
                    // Ensure the script is running from the correct directory
                     dir('ODC-final-project/ansible') {
                        sh '''
                            ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook -i inventory docker_setup.yml
                        '''
                    }
                }
            }
        }
    }
}

