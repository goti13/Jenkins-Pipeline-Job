# Jenkins-Pipeline-Job
**What is a Jenkins Job**

A Jenkins pipeline job is a way to define and automate a series of steps in the software delivery process it ollows you to script and organise your entire build test and deploment. 
Jenkins pipelines enable organizations to define, visualize and execute intricate build test and deployment processes as core. This facilitates the seamless integration of continuous integration and continuous delivery, (CI/CD) practices into software develonment.

**Creating a Pipeline Job**

i. From the dashboard menu on the left side, click on new item

<img width="1439" alt="image" src="https://github.com/user-attachments/assets/cbc2e63e-8eb2-4674-803b-6935c477250b" />

ii. Create a pipeline job and name it "My pipeline job"

<img width="1439" alt="image" src="https://github.com/user-attachments/assets/5c423a6c-0758-47be-bec5-af591a6e0d44" />


**Configure Build Trigger**

i. Click "Configure" your job and add this configurations

ii. Click on build trigger to configure triggering the job from GitHub webhook

<img width="1048" alt="image" src="https://github.com/user-attachments/assets/b266e5e8-11eb-4435-ab9b-57ee911244ba" />


iii. Create a github webhook using jenkins ip address and port

<img width="1178" alt="image" src="https://github.com/user-attachments/assets/f799e949-7ec7-4af9-95db-00e7d596697d" />

Now let's work on our pipeline script

**Writting Jenkins Pipeline Script**

A jenkins pipeline script refers to a script that defines and orchestrates the steps and stages of a continuous integration and continuous delivery (CI/CD) pipeline. Jenkins pipelines can be defined using either declarative or scripted syntax. Declarative syntax is a more structured and concise way to define pipelines. It uses a domain-specific language to describe the pipeline stages, steps
and other configurations while scripted syntax provides more flexibility and is suitable for complex scripting requirements.

**Lets write our pipeline script**

```

pipeline {
    agent any

    stages {
        stage('Connect To Github') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/goti13/jenkins-scm.git']])
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t myapp .'
                }
            }
        }
        stage('Run Docker Container') {
            steps {
                script {
                    sh 'docker run -itd -p 8081:80 myapp'
                }
            }
        }
    }
}



```

**Explanation of the script above**

The provided Jenkins pipeline script defines a series of stages for a continuous integration and continuous delivery (CI/CD) process. 

Let's break down each stage:

**Agent configuration**

```

agent any

```

Specifies that the pipeline can run on any available agent (an agent can either be a jenkins master or node). This means the pipeline is not tied to a specific node type.

**Stages**

```

stages {
   // Stages go here
}

```

Defines the various stages of the pipeline, each representing a phase in the software delivery process.

**Stage1: Connect to github**

```

stage('Connect To Github') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/goti13/jenkins-scm.git']])
            }
        }

```

- This stage checks out the source code from a GitHub repository ('https://github.com/goti13/jenkins-scm.git').
  
- It specifies that the pipeline should use the 'main' branch.

**Stage 2: Build docker image**

```
 stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t myapp .'
                }
            }
        }
```

- This stage builds a Docker image named 'dockerfile' using the source code obtained from the GitHub repository.
- The 'docker build' command is executed using the shell (`sh`)

**Stage 3: Run Docker Container**

stage('Run Docker Container') {
   steps {
      script {
         sh 'docker run -itd --name nginx -p 8081:80 myapp'
      }
   }
}


- This stage runs a Docker container named 'nginx' in detached mode ('-itd').
- The container is mapped to port 8081 on the host machine (*-p 8081:80*).
- The Docker image used is the one built in the previous stage (dockerfle).

**Copy the pipeline script and paste it in the section below**

![image](https://github.com/user-attachments/assets/d7a45cb4-2884-4ad1-bd81-0ebd33e0dfd9)

The stage 1 of the script connects jenkins to github repository. To generate syntax for your github repository, follow the steps below

i. Click on the pipeline syntax

<img width="1132" alt="image" src="https://github.com/user-attachments/assets/faf5555e-b065-44a3-854c-83ba0da454c2" />

ii. Select the drop down to search for 'checkout: Check out from version control'


<img width="1103" alt="image" src="https://github.com/user-attachments/assets/54f9d31a-0b57-41bd-a50e-371a1fb6d1fa" />

iii. Paste the repository url and make sure your branch is main


<img width="1029" alt="image" src="https://github.com/user-attachments/assets/5f23fb67-7d67-4c3f-a474-0f1b38a511b2" />

iv. Generate your Pipeline script

<img width="1112" alt="image" src="https://github.com/user-attachments/assets/d65aa9f7-2ecd-4654-9d78-5e70ed772083" />

Now you can replace the generated script for connect jenkins with github.

**Installing Docker**

Before jenkins can run docker commands, we need to install docker on the same instance jenkins was installed. From our shell scripting knowledge, let's install docker with shell script

i. Create a file named docker.sh

ii. Open the file and paste the script below

```

#!/bin/bash

# Step 1: Update the system
echo "Updating the system..."
sudo yum update -y


# Step 2: install docker 
echo "Downloading and running the Docker installation script..."
sudo dnf install -y docker

# Step 4: Start and enable Docker
echo "Starting and enabling Docker..."
sudo systemctl start docker
sudo systemctl enable docker --now

# Step 5: Verify Docker installation
echo "Verifying Docker installation..."
sudo docker --version
sudo systemctl status docker


echo "Docker installation and setup completed!"

```

NOTE:

The script above is for Amazon linux and centos Stream compactible disto, If you are running ubuntu, below will be the equivalent script:

```

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

```

iii. Save and close the file

iv. Make the file executable

```
chmod u+x docker.sh

```

v. Execute the script

```
./docker.sh

```

**Building Pipeline Script**

Now that we have docker installed on the same instance with jenkins, we need to create a dockerfile before we can run our pipeline script. As we know, we cannot build a docker image without a dockerfile. 

i. Create a new file named 'Dockerfile'

ii. Paste the code snippet below in the file

```

# Use the official NGINX base image
FROM nginx:latest

# Set the working directory in the container
WORKDIR  /usr/share/nginx/html/

# Copy the local HTML file to the NGINX default public directory
COPY index.html /usr/share/nginx/html/

# Expose port 80 to allow external access
EXPOSE 80

```


iii. Create an `idex.html` file and paste the contents below:

```

Congratulations, You have successfully run your first pipeline code.

```


To access the `index.html` file on our web browser, you need to first edit the inbound rules and open the port we mapped our container to( 8081)


<img width="1423" alt="image" src="https://github.com/user-attachments/assets/3023aced-87c3-4d28-b110-6d44bcd2dca8" />


We can now access the content of the web browser

```

http://jenkins-ip-address:8081

```

![image](https://github.com/user-attachments/assets/279123a6-2ef5-4809-90b3-90a3ba31cfe2)

**Project Challenges**

While implementing this project, I encountered `permission denied` issue when I triggered the pipeline. The Jenkins pipeline got "permission denied" while trying to connect to the Docker daemon socket". This means that the Jenkins user does not have the necessary permissions to access Docker.

To fix this issue, I ran the commands:

i. Check if docker group exists:

```
getent group docker

```

If it does not exist, create it:

```

sudo groupadd docker

```

ii. Add the Jenkins user to the docker group:

```
sudo usermod -aG docker jenkins

```




















