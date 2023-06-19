pipeline {
    agent any
    stages{
        stage('Clone the code'){
            steps{
                git branch: 'main', url: 'https://github.com/ShikharChaudhry/ecswebapp.git'
            }
        }
        stage('Login to ECR'){
            steps{
                script{
                sh '''aws ecr get-login-password --region ap-south-1 | docker login --username AWS --password-stdin 971467359615.dkr.ecr.ap-south-1.amazonaws.com'''
                }
            }
        }
        
        stage('Build the image'){
            steps{
                script{
                sh '''docker build -t newtestrepo /var/lib/jenkins/workspace/ecsproject/'''
                }
            }
        }
        stage('tag image and push to ECR'){
            steps{
                script{
                sh '''docker tag newtestrepo:latest 971467359615.dkr.ecr.ap-south-1.amazonaws.com/newtestrepo:latest
                docker push 971467359615.dkr.ecr.ap-south-1.amazonaws.com/newtestrepo:latest'''
                }
            }
        }
        stage('update the taskdefiniton'){
            steps{
                script{
                sh '''aws ecs register-task-definition --cli-input-json file://./taskdef.json --region ap-south-1'''
                }
            }
        }
        stage('update the service'){
            steps{
                script{
                sh '''aws ecs update-service --cluster ECS-demo-Cluster --service ECS_service --force-new-deployment --region ap-south-1 --task-definition ECS-taskdefinition'''
                }
            }
        }
    }
}
