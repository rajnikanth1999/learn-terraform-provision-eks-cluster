pipeline {
    agent any
    triggers {
        pollSCM('* * * * *')
    }
    stages {
        stage('terraform') {
            steps {
                sh '''
                terraform init
                terraform apply --auto-approve
                aws eks --region $(terraform output -raw region) update-kubeconfig --name $(terraform output -raw cluster_name)
                '''
            }
        }
        stage ('kubectl apply') {
            steps {
                sh '''
                kubectl create secret docker-registry regcred --docker-username=praveenrajnikanth --docker-password=N@ni@!999 --docker-email=praveendba31@gmail.com
                kubectl apply -f saleor.yml
                kubectl get all
                '''
            }
        }
    }
}