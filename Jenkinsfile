pipeline {
    agent any
    environment {
        AWS_ACCOUNT_ID="422097883691"
        AWS_DEFAULT_REGION="us-east-1"
    }

    stages {    
    
        stage('Logging into AWS ECR') {
            steps {
                withCredentials([string(credentialsId: 'AWS_SECRET_ACCESS_KEY', variable: 'AWS_SECRET_ACCESS_KEY'), string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID')]) {
                    sh 'aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/flask-app'
                    sh 'aws ecr get-login-password --region ${AWS_DEFAULT_REGION} | docker login --username AWS --password-stdin ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/mysql-db'
                    echo 'logging to aws ECR successed'
                }

            }
        }

    

        stage('Build docker image Flask-app'){
            steps {
                sh 'docker build -t ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/flask-app:latest   ./FlaskApp'
                echo 'Build Flask-app image is successed'
            }
        }

        stage('Build docker image MySQL-database'){
            steps {
                sh 'docker build -t ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/mysql-db:latest ./MySQL_Queries'
                echo 'Build Mysql-db image is successed'
            }
        }

        stage('docker push Flask-app') {
            steps {
                withCredentials([string(credentialsId: 'AWS_SECRET_ACCESS_KEY', variable: 'AWS_SECRET_ACCESS_KEY'), string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID')]) {
                    sh 'docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/flask-app:latest'
                    echo 'Push Flask-app image is successed'
                }
            }
        }

        stage('docker push MySQL-database') {
            steps {
                withCredentials([string(credentialsId: 'AWS_SECRET_ACCESS_KEY', variable: 'AWS_SECRET_ACCESS_KEY'), string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID')]) {
                    sh 'docker push ${AWS_ACCOUNT_ID}.dkr.ecr.${AWS_DEFAULT_REGION}.amazonaws.com/mysql-db:latest'
                    echo 'Push Mysql-db image is successed'
                }
            }
        
        }
        stage('Add EKS kubeconfig') {
            steps {
                withCredentials([string(credentialsId: 'AWS_SECRET_ACCESS_KEY', variable: 'AWS_SECRET_ACCESS_KEY'), string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID')]) {
                    sh "aws eks --region ${AWS_DEFAULT_REGION} update-kubeconfig --name eks_cluster"
                }
            }
        }
              
        stage('install controller') {
            steps {
                withCredentials([string(credentialsId: 'AWS_SECRET_ACCESS_KEY', variable: 'AWS_SECRET_ACCESS_KEY'), string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID')]) {
                    sh 'kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.6.4/deploy/static/provider/aws/deploy.yaml'
                }
            }
        }
        stage('Deploy k8s mainfest') {
            steps{
                withCredentials([string(credentialsId: 'AWS_SECRET_ACCESS_KEY', variable: 'AWS_SECRET_ACCESS_KEY'), string(credentialsId: 'AWS_ACCESS_KEY_ID', variable: 'AWS_ACCESS_KEY_ID')]) {
                    sh 'kubectl apply -f ./kubernates/.'
                }
            }
        }
        
    }
}
        
// stage('Add EKS kubeconfig') {
//             steps {
//                 sh "aws eks --region ${AWS_DEFAULT_REGION} update-kubeconfig --name eks_cluster"
//             }
//         }
        // stage('install nginx controller') {
        //     steps {
                
        //         sh "kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.6.4/deploy/static/provider/aws/deploy.yaml"
        //     }
        // }
        // stage('k8s-deploy-ingress-service') {
        //     steps {
        //         withKubeCredentials(kubectlCredentials: [[caCertificate: '', clusterName: 'k8s', contextName: '', 
        //            credentialsId: 'kubeconfig', namespace: '', serverUrl: 'https://172.31.21.73:6443']]) {
                
        //                     sh 'kubectl apply -f flaskapp-deployment.yml'
        //                     sh 'kubectl get pods '
        //         } 
        //     }
        // }

    // stage('Deploying app to k8s') {
    //   steps {

    //     sshagent(['k8s']) {
    //       sh "ssh -o StrictHostKeyChecking=no ansadmin@35.180.66.25 "
    //       //sh "'scp /var/lib/jenkins/workspace/projet9/deployment-tomcat-to-k8s.yml ansadmin@35.180.66.25:./'"
    //       sh "ssh -o StrictHostKeyChecking=no ansadmin@35.180.66.25  'ls'"

    //       script {
    //           sh "ssh ansadmin@35.180.66.25 kubectl create -f ."
    //       }

    //     }
    //   }
    // }  echo -n "password" |base64
    // kubectl exec -n test-ebs -ti $MYSQLPOD -- mysql --user=root --password=password
    // kubectl exec  -ti mysql-779df889dd-rkppb  -- mysql --user=dev --password=dev


