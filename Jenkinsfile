pipeline {
    agent { label 'netligent' }

    environment {
        IMAGE_NAME = "myapp"
        IMAGE_TAG = "${BUILD_NUMBER}"
        CONTAINER_NAME = "myapp-container"
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
                    inventory: 'ansible/inventory',
                    become(true),
                    becomeUser("user"),
                    extras: """
                      --extra-vars
                      image_name=${IMAGE_NAME}
                      image_tag=${IMAGE_TAG}
                      container_name=${CONTAINER_NAME}
                    """,
                    colorized: true,
                    disableHostKeyChecking: true,
                    installation: 'ansible-2.14'
                )
            }
        }
    }
}
