# Jenkins CI/CD with GitHub Integration

## Pre-requisities:

* AWS Account
* GitHub Account
* Basic Knowledge about Docker & Jenkins

### Steps:-



#### 1. Launch an EC2 Ubuntu Instance with access of HTTP & HTTPS
#### 2. Install Jenkins on Ubuntu Machine
#### 3. Setup Jenkins & Connect with GIT by Installing Github Integration
#### 4. Finally Enable the Jenkins Script for CI/CD of Docker Provision Node.js
#### 5. Install Github integration Plugin
#### 6. Webhook Configuration



### Step 1:

**Go to your AWS Account & Launch an Ubuntu Instance**

Open EC2 -> Instances --> Launch an EC2 Instance
Select Ubuntu Image**

![image](https://user-images.githubusercontent.com/84725860/210175095-58852d0b-fbdf-431f-9991-5d12fd35647f.png)

* Provide Following Details :

Name = Jenkins-Project-1

Instance Type = t2.micro

Allow HTTP & HTTPS Traffic From Internet

Creat New Key Pair

Once Done Launch Your EC2 Instance

Note: Please Create a new key pair or use existing one to login


### Step 2: 

**Connect to your EC2 Instance to Install Jenkins **

Run below Commands One by One

Update Your System

```
sudo apt update
```

Install JAVA

```
sudo apt install openjdk-11-jre
```

Check JAVA Version by running below command

```
java -version
```

Now Install Jenkins

```
curl -fsSL https://pkg.jenkins.io/debian/jenkins.io.key | sudo tee \   /usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

```
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \   https://pkg.jenkins.io/debian binary/ | sudo tee \   /etc/apt/sources.list.d/jenkins.list > /dev/null
```

```
sudo apt-get update 
```

```
sudo apt-get install jenkins
```

Once Done Enable & Start the Jenkins Service


```
sudo systemctl enable jenkins
```

Starts the Jenkins Service

```
sudo systemctl start jenkins
```

Check the Service Status

```
sudo systemctl status jenkins
```


Now Goto Security Group of your EC2 Instance & provide below Inbound Rules & Save Changes

![image](https://user-images.githubusercontent.com/84725860/210175715-6523bf2e-306d-4e0c-bf21-cdfe23b48500.png)

Now Open your Jenkins Server by Below Address

```
http:// [public-ip]:8080 /
```

You can see below screen 

![image](https://user-images.githubusercontent.com/84725860/210175757-b4d812cf-0c5b-43d4-ae82-58a6f014a113.png)


### Step 3:

**Now Locate your Jenkins Administrator password by command**


```
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```


Enter that password & select Install Suggested Plugins Once Done Provide the Necessary Details & click on Save & Continue


Check the Jenkins URL & click on Save & Finish


Now Click on Start Using Jenkins you can see below screen 

![image](https://user-images.githubusercontent.com/84725860/210176171-23f7895e-150f-41fc-a8d9-c4174a5e4b97.png)

Provide Item Name, we are using freestyle pipeline so choose freestyle project

![image](https://user-images.githubusercontent.com/121545847/210228201-a68aab91-a0a5-4885-a066-012c680f455d.png)

Once Done Click on save

Before Configuring lets connect Jenkins with GIT using SSH


Go to your EC2 Instance & run below command

```
ssh-keygen
```

It will generate public & private key provide public key to GitHub & provide private key for Jenkins
Access the keys changing the directory to

```
cd .ssh
ls
```


Now Go to your GitHub & provide the public key as

```
cat id_rsa.pub
```

![image](https://user-images.githubusercontent.com/84725860/210176867-19c1cbd6-9ce6-4e91-aa26-007a3341c548.png)

Copy that key and add to your GitHub SSH Keys Section


As you can see i have added the SSH key for the GitHub
Now similarly add private key to your Jenkins
Go to your Project -->  Configure

Check Github Project & Provide the Project URL

![image](https://user-images.githubusercontent.com/121545847/210234072-3ecd07f1-5b6a-4162-b87e-a93924484b54.png)



In Source Code Management select GIT and paste the repository URL 


![image](https://user-images.githubusercontent.com/84725860/210176808-a6e4023d-0322-40fc-830b-c72473590105.png)


Now in credentials click on add

Provide the Details
In kind select SSH with Private Key
Username select as Ubuntu  -->  Username of EC2 Instance

In Private key select Entire Directly & paste your Private Key as we copied public key

```
cat id_rsa
```

Once Done Check Specifier For me it's main

![image](https://user-images.githubusercontent.com/84725860/210177246-cf8a51c2-3679-4d15-be93-bb5a07515762.png)

Now click on Save

After that click on Build Now

You can see build is started Once Done open that build 

Go to console output & copy the address

![image](https://user-images.githubusercontent.com/121545847/210234224-4a72ab1e-2ba0-4cd2-a50c-6b76a2aeaf6f.png)



Now open your Instance & change Directory with

```
cd /var/lib/jenkins/workspace/Item-Name  (Pranoti_G is my Item Name)
```

Installing Node. js and npm from NodeSource

```
sudo apt install nodejs sudo apt install npm
```

Install the  requirement for .json

```
npm install
```
Install dependencies and then start the app

```
node app.js
```

Now Your App is running on 8000 Port so give the inbound accesss:

![image](https://user-images.githubusercontent.com/84725860/210177628-90d0e1f1-094c-4edc-82f2-b74e57172da3.png)

![image](https://user-images.githubusercontent.com/84725860/210177652-d210d67f-c09f-4167-9eb9-fde2b8b6aa66.png)


Type the Public ip with 8000 Port:- 

![image](https://user-images.githubusercontent.com/121545847/210234319-3425b5fc-7610-485a-ac06-b418f89a7561.png)


Now lets install docker and build the docker image by following commands 


```
sudo apt install docker.io
```

Now creat the docker file 

```
vim dockerfile
```

![image](https://user-images.githubusercontent.com/84725860/210178076-2534550d-ea81-46d7-ad7e-cd1e4e48aef3.png)


Give the commands in that docker file

Install Alpine Linux image with node 12.2.0 

```
From node:12.2.0-alpine
```
Define the working directory

```
WORKDIR app
```
Copy all the files from the project's root to /app

```
COPY . .
```
Install node_modules

```
RUN npm install
```
This tells Docker your webserver will listen on port 8080

```
EXPOSE 8080
```
CMD command specifies the instruction that is to be executed when a Docker container starts

```
CMD ["node","app.json"]
```

![image](https://user-images.githubusercontent.com/84725860/210178326-c5bdae65-b710-42c5-8c7d-8725225df829.png)

Add your user to the docker group / If you want to be able to skip sudo for docker commands

```
sudo usermode -a -G docker $USER
```

Reboot the docker user

```
sudo reboot
```

Launch the instance and got to Build path
Once Rebooted build Image by following Command 

```
sudo docker build . -t todo-job1
```
After Successfully image is built run the image by

```
sudo docker run -d  --name todo-job1 -p 8000:8000 todo-job1
```

Here Container-name --> todo , -d --> detached mode , -p -->Expose port 8000

To check the state of Docker

```
docker ps
```


### Step 4:-


**Now lets Run the Docker through Jenkins**

Give Full Access to Build Path (Pranoti_G is my Item Name)


```
sudo chmod 777 /var/lib/jenkins/workspace/Pranoti_G 
```

To Connect Docker Domain Socket

```
sudo usermod -a -G docker jenkins
```

Then Restart Our Jenkins

```
sudo systemctl restart jenkins
```

Then add the Build Steps in Execute Shell

![image](https://user-images.githubusercontent.com/121545847/210248806-f21e2c98-45bb-4f48-9e6d-cb9e636139f8.png)

Now check the Port 8000 is running Successfully

![image](https://user-images.githubusercontent.com/121545847/210249093-01ded4b3-5a7e-4350-add7-36fdb96a3dfc.png)

### Step 5:

**Install Github integration Plugin**

![image](https://user-images.githubusercontent.com/121545847/210249347-96a99fc1-9407-42e5-8bdd-921042c32740.png)

### Step 6:

**Webhook Configuration**

In GitHub SSH OR GPG Key Should Be Present 
then

Go to Webhooks which is in Repo Settings and add webhook

![image](https://user-images.githubusercontent.com/121545847/210254199-b1117352-8476-4274-a365-754a26581a35.png)


then Go to jenkins and configure the Build triggers

![image](https://user-images.githubusercontent.com/121545847/210254368-c42275c0-8471-4d9d-b99d-3856e0d63e03.png)

Now Change in Github you will see automatic deployment will there


![image](https://user-images.githubusercontent.com/121545847/210254649-a5ec1fb6-f58b-4a36-aa8a-70da7608eb90.png)


# Thank You







  





