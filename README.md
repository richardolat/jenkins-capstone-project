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




#### Create a freestyle Project for a web application 
































































