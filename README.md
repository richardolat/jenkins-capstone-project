# jenkins-capstone-project
This jenkins project is to design and implement a robust CI/CD pipeline using Jenkins to automate the deployment of a web application. The goal is to achieve continuous integration, continuous deployment, and ensure the scalability and reliability of the applications.




#### Create An AWS instance as a dedicated server for jenkins
![image](https://github.com/user-attachments/assets/92bc3cc9-c8e9-4648-a66d-2cf5802275fd)


#### Update the package repositories 
sudo apt update
![Screenshot 2024-11-16 011624](https://github.com/user-attachments/assets/93120e53-9990-4af6-8b83-7dd7147a92da)


#### Install java "JDK" as a pre-requisite for installing jenkins
sudo apt install openjdk-17-jre
![image](https://github.com/user-attachments/assets/0659d1d8-9a30-433a-85ac-77d8d5fbfed6)


#### Install jenkins 
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io-2023.key | sudo tee
/usr/share/keyrings/jenkins-keyring.asc > /dev/null echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]
https://pkg.jenkins.io/debian binary/ | sudo tee
/etc/apt/sources.list.d/jenkins.list > /dev/null sudo apt-get update sudo apt-get install jenkins
![image](https://github.com/user-attachments/assets/f8beabe0-3f69-4a38-8d73-b40c68b12828)



#### Confirm installation and status of jenkins
sudo systemctl status jenkins 
![image](https://github.com/user-attachments/assets/4fe370fd-d8b8-4554-a638-a74d985bf1fe)




#### In the Jenkins dedicated server "instance", create new inbound rule for port 8080 in the security group
By default Jenkins listens on port 8080
![image](https://github.com/user-attachments/assets/6a9fb347-0e73-4126-974b-c875af39aca6)



#### Set up Jenkins on Web console
i. Input jenkins instance ip address on the web browser ii. On the Jenkins server, run "sudo cat /var/lib/jenkins/secrets/initialAdminPassword" to get the password
![Screenshot 2024-11-16 022134](https://github.com/user-attachments/assets/13fc12db-f2f8-4ae7-bdb1-6e12f373a2b2)
![image](https://github.com/user-attachments/assets/a581968a-661b-4380-8343-c9561c020df3)



#### Install suggested plugins
![image](https://github.com/user-attachments/assets/e7c94e13-2e7c-4190-aa95-2521593f9208)
![image](https://github.com/user-attachments/assets/132872a6-7404-4a99-91f8-2e18219f145c)



#### Create a user account 
![image](https://github.com/user-attachments/assets/42eafb53-bf70-4ecf-af73-7100b5d8a681)



#### Log in into jenkins
![image](https://github.com/user-attachments/assets/365c3308-f69e-4c01-9a7c-f57dc9e92930)



#### Create Freestyle job for web application
Select New Item 
Enter the project name "ecommerce-webapp" and click in freestyle project
![Screenshot 2024-11-16 035005](https://github.com/user-attachments/assets/ecdc2ea7-3616-49ce-9e82-6420138c3ec3)


#### Integrate Jenkins with Source Code Management Repository 
choose Git and Provide the github repository url 
![Screenshot 2024-11-16 040010](https://github.com/user-attachments/assets/b66b3428-88d3-49d3-b33c-ea3fdb3ab8a1)


#### Configure Webhook for automatic trigerring of jenkins build
click on triggers under the source code management in jenkins console and click Github hook trigger for GitsM polling
![Screenshot 2024-11-16 040718](https://github.com/user-attachments/assets/55e35323-d27d-479a-a35c-df618d21b440)




#### On the github, go to settings, go to webhook and paste the jenkins url in the provided space 
![image](https://github.com/user-attachments/assets/48e9275b-373f-49b2-a338-077b7f965401)




#### Build the job 
![image](https://github.com/user-attachments/assets/cc197329-7c3f-4feb-bbfa-53cfe3aecd43)



## Jenkin Pipeline for Web Application 


#### Develop a Jenkins pipeline for running a web application
Click New item. Give the project name and click pipeline 
![image](https://github.com/user-attachments/assets/aebd53e6-9d6a-4cf6-a724-fecaab6d4d2b)




#### Configure the build trigger to automatically trigger build from github
![image](https://github.com/user-attachments/assets/6e834493-872e-4334-8ca8-b95ed5b55618)



#### Provide a pipeline script to run the web application
pipeline {
    agent any

    stages {
        stage('Connect To Github') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/richardolat/jenkins-capstone-project.git']])
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t dockerfile .'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    sh 'docker run -itd --name nginx -p 8081:80 dockerfile'
                }
            }
        }
    }
}

![Screenshot 2024-11-16 043833](https://github.com/user-attachments/assets/1e7868c0-094b-4efd-ac68-40fee5eff27d)




#### Click on Pipleline syntax
![image](https://github.com/user-attachments/assets/f7d41228-ce47-4d6d-85e0-4c4ab772b08c)




#### From the drop-down, select "checkout: checkout from version control"
![Screenshot 2024-11-16 044613](https://github.com/user-attachments/assets/184744cf-516e-421f-a216-438984ea85f7)


#### Automate the Creation of a docker image and push to the registry
create a file named "docker.sh" to install docker 

sudo apt-get update -y
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update -y
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y
sudo systemctl status docker


#### Make the file executable 
chmod u+x docker.sh
![Screenshot 2024-11-16 050246](https://github.com/user-attachments/assets/86fdea1d-be0e-4f9e-a190-15d4dbc6e4b4)



#### Execute the file 
./docker.sh
![image](https://github.com/user-attachments/assets/971a156e-cd40-4bf1-9523-ed7a84fd3ad4)

### Docker is successfully installed 

#### To create a docker image, create a docker file
![image](https://github.com/user-attachments/assets/0ebe9e7c-6c0b-40d3-b607-0ff6c2710b6d)


#### Create index.html file. Customize it to your choice.
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>RICHARD-DAREY.IO SERVICES - E-Commerce Web Application</title>
</head>

<body>
    <header>
        <h1>Welcome to RICHARD-DAREY.IO SERVICES E-Commerce</h1>
    </header>

    <main>
        <section>
            <h2>Featured Products</h2>
            <ul>
                <li>Product 1</li>
                <li>Product 2</li>
                <li>Product 3</li>
            </ul>
        </section>

        <section>
            <h2>About Us</h2>
            <p>RICHARD-DAREY.IO SERVICES is your one-stop destination for all your e-commerce needs. We offer a wide range of products at competitive prices.</p>
        </section>
    </main>

    <footer>
        <p>&copy; 2024 RICHARD-DAREY.IO SERVICES. All rights reserved.</p>
    </footer>
</body>

</html>

#### Push the dockerfile and the index.html file file to the github repository 
![image](https://github.com/user-attachments/assets/09ffbab7-aaf5-4084-8196-eb9fcc24bb04)




#### Add docker to Jenkins group so it can execute the container 
![image](https://github.com/user-attachments/assets/929b7fdc-8922-4220-b406-64c03ee03af9)



#### Any push to the github repository will trigger automatic build of the pipleline job
![Screenshot 2024-11-16 142128](https://github.com/user-attachments/assets/fda00415-e98c-4171-84f9-48bb4d96f5eb)



#### To access the application on the web browser, edit the inbound rule to add port 8081 as configured in the dockerfile 
![image](https://github.com/user-attachments/assets/fa8d4a0e-dabc-4f84-aa60-1ad065ec12cb)



#### Paste the jenkins server IP address and port 8081 on the web browser to access the web app content 
![image](https://github.com/user-attachments/assets/dfdbe7e7-c1a9-49f1-bf9a-babed582c564)


#### Push the docker image to the dockerhub repository 
Tag the repo "sudo docker tag dockerfile richardolat/my-docker-image:latest"

![Screenshot 2024-11-16 145716](https://github.com/user-attachments/assets/654703c1-1c06-47f0-bddf-5b00952d60d9)

The docker image is pushed to docker hub repo.


































































































































