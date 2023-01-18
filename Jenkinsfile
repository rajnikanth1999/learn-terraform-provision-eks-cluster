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
                kubectl get nodes
                kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.4.0/aio/deploy/recommended.yaml
                kubectl apply -f eks-admin-service-account.yaml
                kubectl -n kube-system describe secret $(kubectl -n kube-system get secret | grep eks-admin | awk '{print $1}')
                kubectl proxy
                '''
            }
        }
    }
}