# Build and Deploy a Simple Web Application on Target Machines Using a CI Pipeline

## Overview
This project demonstrates how to build and deploy a Python web application on two target machines using a CI pipeline. The process involves containerizing the application, configuring infrastructure, and automating deployment using Jenkins and Ansible. Notably, the pipeline underwent 70 iterations of failure before succeeding, showcasing the persistence and iterative improvement required in real-world CI/CD workflows.

---

## Steps to Implement the Task

### 1. Push the Python Code to GitHub Repository
- Clone the repository:
  ```bash
  git clone <repository_url>
  ```
- Add the provided Python code to the repository.
- Push the changes:
  ```bash
  git add .
  git commit -m "Add initial Python application code"
  git push origin main
  ```

### 2. Write and Push a Dockerfile
- Create a `Dockerfile` in the root of your repository:
  ```dockerfile
  # Use an official Python runtime as a parent image
  FROM python:3.9-slim

  # Set the working directory in the container
  WORKDIR /app

  # Copy the current directory contents into the container
  COPY . /app

  # Install any dependencies specified in requirements.txt
  RUN pip install --no-cache-dir -r requirements.txt

  # Make port 5000 available to the world outside the container
  EXPOSE 5000

  # Run the application
  CMD ["python", "app.py"]
  ```
- Push the `Dockerfile` to your repository:
  ```bash
  git add Dockerfile
  git commit -m "Add Dockerfile"
  git push origin main
  ```

### 3. Create Two Vagrant Machines
- Initialize a Vagrantfile:
  ```bash
  vagrant init
  ```
- Define two low-spec machines in the Vagrantfile:
  ```ruby
  Vagrant.configure("2") do |config|
    config.vm.define "machine1" do |machine1|
      machine1.vm.box = "ubuntu/bionic64"
      machine1.vm.network "private_network", type: "dhcp"
    end

    config.vm.define "machine2" do |machine2|
      machine2.vm.box = "ubuntu/bionic64"
      machine2.vm.network "private_network", type: "dhcp"
    end
  end
  ```
- Start the machines:
  ```bash
  vagrant up
  ```

### 4. Set Up the Jenkins Pipeline
#### Configure Jenkins
- Install necessary Jenkins plugins (e.g., Git, Docker Pipeline, Ansible).

#### Define Jenkins Pipeline Steps
1. **Pull Code from GitHub**
   - Add repository credentials.
   - Use the `Checkout SCM` step in the Jenkinsfile.

2. **Build Docker Image**
   ```groovy
   stage('Build Docker Image') {
       steps {
           script {
               docker.build('your-image-name')
           }
       }
   }
   ```

3. **Push Docker Image to Docker Hub**
   ```groovy
   stage('Push to Docker Hub') {
       steps {
           script {
               docker.withRegistry('https://registry.hub.docker.com', 'docker-hub-credentials') {
                   docker.image('your-image-name').push()
               }
           }
       }
   }
   ```

4. **Run Ansible Playbook**
   - Use an Ansible playbook to:
     1. Install Docker on the Vagrant machines.
     2. Pull the Docker image from Docker Hub.
     3. Run a container from the image.

#### Example Jenkinsfile
```groovy
pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/your-repo.git'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    docker.build('your-docker-image')
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://index.docker.io/v1/', 'docker-hub-credentials') {
                        docker.image('your-docker-image').push('latest')
                    }
                }
            }
        }
        stage('Run Ansible Playbook') {
            steps {
                ansiblePlaybook credentialsId: 'ansible-credentials', inventory: 'inventory_file', playbook: 'playbook.yml'
            }
        }
    }
}
```

### 5. Write the Ansible Playbook
#### Inventory File (`inventory_file`)
```ini
[target]
192.168.56.101 ansible_user=vagrant ansible_ssh_private_key_file=/path/to/private/key
192.168.56.102 ansible_user=vagrant ansible_ssh_private_key_file=/path/to/private/key
```

#### Playbook (`playbook.yml`)
```yaml
- hosts: target
  become: true
  tasks:
    - name: Install Docker
      apt:
        name: docker.io
        state: present
    - name: Pull Docker Image
      command: docker pull your-docker-image:latest
    - name: Run Docker Container
      command: docker run -d -p 8080:5000 your-docker-image:latest
```
6. Run the Pipeline

<<<<<<< HEAD
Trigger the Jenkins pipeline manually or schedule it based on your requirement.

Verify each stage completes successfully by reviewing the Jenkins console output.

### Confirm that the application is running on both target machines by accessing the exposed port
 ```(e.g., http://<machine_ip>:8080).```
=======
### 6. Run the Pipeline
- Trigger the Jenkins pipeline manually or schedule it based on your requirement.
- Verify each stage completes successfully by reviewing the Jenkins console output.
- Confirm that the application is running on both target machines by accessing the exposed port (e.g., `http://<machine_ip>:8080`).

---

## Pipeline Iterations
The pipeline execution journey involved 70 iterations of failure before achieving success. This process emphasized the importance of debugging, incremental improvements, and persistence in CI/CD pipelines.

## Visuals
### 1. Jenkins Pipeline Overview
![Jenkins Pipeline Overview](path/to/jenkins_pipeline_overview.png)

### 2. Docker Containers Running
![Docker Containers Running](path/to/docker_containers_running.png)

### 3. Application Accessible on Target Machines
![Application Running](path/to/application_running.png)

---

## Conclusion
This task automates the CI/CD pipeline to containerize and deploy a Python application. Each step ensures seamless integration and deployment of the application on the target machines using tools like Jenkins, Docker, Vagrant, and Ansible.


