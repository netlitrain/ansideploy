pipeline {
    agent { label 'netligent' }

    environment {
        IMAGE_NAME = "myapp"
        IMAGE_TAG = "${BUILD_NUMBER}"
        CONTAINER_NAME = "myapp-container"
        ANSIBLE_SSH_ID = "ubuntukikey"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Deploy via Ansible Plugin') {
            steps {
                ansiblePlaybook(
                    playbook: 'ansible/deploy.yml',
                    inventory: 'ansible/hosts.ini',
                    credentialsId: '${ANSIBLE_SSH_ID}',
                    extras: """
                      --extra-vars image_name=${IMAGE_NAME} image_tag=${IMAGE_TAG} container_name=${CONTAINER_NAME}
                    """,
                    colorized: true,
                    disableHostKeyChecking: true,
                    installation: 'ansible-2.14'
                )
            }
        }
    }
}
