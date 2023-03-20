# MySQL-and-Python
A small web app that allow users to create account, login, and make a bucket list.
This app is implemented using MySQL and Python. Copied from the tutorial http://code.tutsplus.com/tutorials/creating-a-web-app-from-scratch-using-python-flask-and-mysql--cms-22972
# MySQL-and-Python
## <font size=”20”> **Provision AWS Infrastructure using Terraform** </font>

- EC@ (for jenkins)
- EKS (for cluster)
- ECR ( two repo one for application and other for database)
- 
 Commands to run terraform:
 ```
 $ cd terraform
 ```
 ```
 $ terraform init
 ```
 ```
 $ terraform apply --var-file vars.tfvars -auto-approve
 ```
 output:
 
 ![image](https://user-images.githubusercontent.com/71265897/226346888-f35d8a8a-0e50-4487-b0b8-415bc509d7f6.png)
 
 Take the jenkins-ip and replace it with inventory file:
 
 ![image](https://user-images.githubusercontent.com/71265897/226341875-ef93d82c-2fb3-4aff-97a5-f2c1f88b93d6.png)

## <font size=”20”> **Install jenkins and awscli kubectl using ansible ** </font>
 Commands to run terraform:
 ```
 $ cp jenkins_key.pem ../ansible/
 ```
 ```
 $ cd ../ansible/
 ```
 ```
 $ ansible-playbook -i inventory --private-key jenkins_key.pem web.yml
 ```
 output:
 
 copy the initial password and paste it when you open jenkins-ip:8080:
 
