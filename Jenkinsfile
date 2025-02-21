pipeline {
    agent {label "docker-build-node"}
    environment {
        AWS_DEFAULT_REGION = 'ap-south-1'
        EKS_CLUSTER_NAME = 'gdl-eks-cluster'
        //DOCKER_HOST = 'tcp://localhost:2375' // For Windows
        // DOCKERHUB_CREDENTIALS = credentials('DOCKERHUB_CREDENTIALS')
        DOCKER_PRIVATE_REPO_IMAGE = 'gdlimbani/smartpps-frontend:20241101'
        DOCKER_PUBLIC_REPO_IMAGE = 'gdlimbani/smartpps-backend:20241101'
    }
    stages {
        // stage('Initialize') {
        //     steps {
        //         script {
        //             // Install Terraform if not already installed
        //             bat '''
        //             terraform --version || \
        //             curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg && \
        //             sudo apt update && \
        //             sudo apt install terraform
        //             '''
        //         }
        //     }
        // }
        // stage('Checkout') {
        //     steps {
        //         git 'https://github.com/gdlimbani/Terraform-jenkins-aws-eks.git'
        //     }
        // }
        // stage('Build Docker Images') {
        //     steps {
        //         script {
        //             docker.build("frontend", "./frontend")
        //             docker.build("backend", "./backend")
        //         }
        //     }
        // }
        // stage('Push Docker Images') {
        //     steps {
        //         script {
        //             withDockerRegistry(credentialsId: 'docker-credentials', url: 'https://your-docker-registry') {
        //                 docker.image("frontend").push("latest")
        //                 docker.image("backend").push("latest")
        //             }
        //         }
        //     }
        // }
        // stage('Load Docker Images') {
        //     steps {
        //         script {
        //             // Load local Docker images from tar files
        //             bat '''
        //             docker load -i C:\\path\\to\\frontend-image.tar
        //             docker load -i C:\\path\\to\\backend-image.tar
        //             '''
        //         }
        //     }
        // }
        // stage('Use Local Docker Images') {
        //     steps {
        //         script {
        //             def frontendImage = docker.image("frontend:latest")
        //             def backendImage = docker.image("backend:latest")
        //             frontendImage.run('-p 80:80')
        //             backendImage.run('-p 8080:8080')
        //         }
        //     }
        // }
        // stage('Login to Docker Hub') {
        //     steps {
        //         bat '''
        //         echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin
	    //         echo 'Login Completed'
        //         '''
        //     }
        // }

        stage('Pull Docker Images') {
            steps {
                bat '''
                docker pull $DOCKER_PRIVATE_REPO_IMAGE
                docker pull $DOCKER_PUBLIC_REPO_IMAGE
                '''
            }
        }
        stage('Deploy to EKS') {
            steps {
                bat '''
                kubectl apply -f frontend-deployment.yaml
                kubectl apply -f backend-deployment.yaml
                kubectl apply -f frontend-service.yaml
                kubectl apply -f backend-service.yaml
                '''
            }
        }
    }
    post {
        always {
            bat 'docker logout'
            cleanWs() // Clean up workspace
        }
    }
}
