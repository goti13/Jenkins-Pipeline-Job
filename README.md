# Jenkins-Pipeline-Job
**What is a Jenkins Job**

A Jenkins pipeline job is a way to define and automate a series of steps in the software delivery process it ollows you to script and organise your entire build test and deploment. 
Jenkins pipelines enable organizations to define, visualize and execute intricate build test and deployment processes as core. This facilitates the seamless integration of continuous integration and continuous delivery, (CI/CD) practices into software develonment.

**Creating a Pipeline Job**

i. From the dashboard menu on the left side, click on new item

<img width="1439" alt="image" src="https://github.com/user-attachments/assets/cbc2e63e-8eb2-4674-803b-6935c477250b" />

ii. Create a pipeline job and name it "My pipeline job"

<img width="1325" alt="image" src="https://github.com/user-attachments/assets/ae485151-ecc3-4933-b529-76ad3b6d867d" />


**Configure Build Trigger**

i. Click "Configure" your job and add this configurations
ii. Click on build trigger to configure triggering the job from GitHub webhook

<img width="1261" alt="image" src="https://github.com/user-attachments/assets/72a0b8b3-b832-4793-bce1-0717d6d93f2c" />

iii. Create a github webhook using jenkins ip address and port

<img width="1178" alt="image" src="https://github.com/user-attachments/assets/f799e949-7ec7-4af9-95db-00e7d596697d" />

Now let's work on our pipeline script

**Writting Jenkins Pipeline Script**

A jenkins pipeline script refers to a script that defines and orchestrates the steps and stages of a continuous integration and continuous delivery (CI/CD) pipeline. Jenkins pipelines can be defined using either declarative or scripted syntax. Declarative syntax is a more structured and concise way to define pipelines. It uses a domain-specific language to describe the pipeline stages, steps
and other configurations while scripted syntax provides more flexibility and is suitable for complex scripting requirements.

**Lets write our pipeline script**

```

pipeline \{
    agent any

    stages \{
        stage('Connect To Github') \{
            steps \{
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/goti13/jenkins-scm.git']])
            \}
        \}
        stage('Build Docker Image') \{
            steps \{
                script \{
                    sh 'docker build -t dockerfile .'
                \}
            \}
        \}
        stage('Run Docker Container') \{
            steps \{
                script \{
                    sh 'docker run -itd -p 8081:80 dockerfile'
                \}
            \}
        \}
    \}
\}

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

stages \{
   // Stages go here
\}

```

Defines the various stages of the pipeline, each representing a phase in the software delivery process.

**Stage1: Connect to github**

```

stage('Connect To Github') \{
   steps \{
      checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/goti13/jenkins-scm.git']])
   \}
\}

```

- This stage checks out the source code from a GitHub repository ('https://github.com/goti13/jenkins-scm.git').
  
- It specifies that the pipeline should use the 'main' branch.

**Stage 2: Build docker image **

- This stage builds a Docker image named 'dockerfile' using the source code obtained from the GitHub repository.
- The 'docker build' command is executed using the shell (`sh`)

**Stage 3: Run Docker Container**

stage('Run Docker Container') \{
   steps \{
      script \{
         sh 'docker run -itd --name nginx -p 8081:80 dockerfile'
      \}
   \}
\}


- This stage runs a Docker container named 'nginx' in detached mode ('-itd').
- The container is mapped to port 8081 on the host machine (*-p 8081:80*).
- The Docker image used is the one built in the previous stage (dockerfle).

**Copy the pipeline script and paste it in the section below**











