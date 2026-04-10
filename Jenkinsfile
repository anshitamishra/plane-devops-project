pipeline {
    agent any

    environment {
        KUBECONFIG = '/var/jenkins_home/.kube/config'
    }

    stages {

        stage('Clone Deployment Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/anshitamishra/plane-devops-project'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh '''
                kubectl apply -f deployment.yaml --validate=false
                kubectl apply -f service.yaml --validate=false
                '''
            }
        }

        stage('Check Pods') {
            steps {
                sh 'kubectl get pods'
            }
        }

        stage('Clone AI Assistant Repo') {
            steps {
                dir('assistant') {
                    git branch: 'main', url: 'https://github.com/anshitamishra/ai-devops-debugging-assistant'
                }
            }
        }

        stage('Run AI Debugging Assistant') {
            steps {
                dir('assistant') {
                    sh '''
                    POD=$(kubectl get pods --no-headers | grep plane-worker | awk '{print $1}' | head -n 1)
                    echo "Selected Pod: $POD"
                    python3 main.py --pod $POD
                    '''
                }
            }
        }
    }
}