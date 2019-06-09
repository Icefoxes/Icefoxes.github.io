---
layout:       post
title:        "CICD pipeline(Jenkins) for angular 8 projects on Linux"
subtitle:     ""
date:         2019-06-08 19:45:00
author:       "Kuo"
header-img:   "img/post-bg-miui6.jpg"
header-mask:  0.3
catalog:      false
multilingual: false
comments:     true
tags:
    - Angular
    - Jenkins
---
- Enviroment:  Ubuntu 18.04.2 LTS

### 1. create a new user
* Login into root
```bash
$ ssh root@host_ip
```
* Use the `adduser` command to add a new user
```bash
$ adduser kuo 
```
* Set and confirm the new user's password at the prompt. 
```bash
Set password prompts:
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
```
* Follow the prompts to set the new user's information. It is fine to accept the defaults to leave all of this information blank.
```bash
User information prompts:
Changing the user information for username
Enter the new value, or press ENTER for the default
    Full Name []:
    Room Number []:
    Work Phone []:
    Home Phone []:
    Other []:
Is the information correct? [Y/n]
```
* Use the `usermod` command to add the user to the sudo group.
```bash
$ usermod -aG sudo kuo
```

* Login into new user
```bash
$ su kuo
```
### 2. Install Docker
```
$ curl -fsSL https://get.docker.com -o get-docker.sh
$ sudo sh get-docker.sh
```
* run docker as non-root user
```bash
$ sudo usermod -aG docker kuo
```
* logout

### 3. Install Jenkins by docker
```bash 
$ sudo mkdir -p /var/jenkins
$ sudo chown kuo /var/jenkins  
$ sudo chmod 775 /var/jenkins
$ docker run -d --rm \
  -u root \
  -p 8080:8080 \
  -v /var/jenkins:/var/jenkins_home \
  -v /var/run/docker.sock:/var/run/docker.sock \
  -v "$HOME":/home jenkinsci/blueocean
```
* after jenkins gets started, then login into jenkins server
![](../img/Angular/homepage.png) you will see this
```bash
# get container id
$ docker ps -a
output 
CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                                              NAMES
d69b9ba9acfc        jenkins             "/bin/tini -- /usr/l…"   18 minutes ago      Up 18 minutes       0.0.0.0:8080->8080/tcp, 0.0.0.0:50000->50000/tcp   epic_pare

$ docker exec -it d69b9ba9acfc bash
$ cd /var/jenkins_home/secrets
```
* Install plugin by default
* Set admin user/password

### 4. Create Pipeline
create Jenkinsfile

since ng is not installed globally,  we can use our local @angular/cli, so in package.json, we can use the npm commands.
```javascript
  "scripts": {
    "ng": "ng",
    "build": "node ./node_modules/@angular/cli/bin/ng build --prod --aot",
    "test":  "node ./node_modules/@angular/cli/bin/ng test"
```
```javascript
pipeline {
    agent {
        docker { image 'node:10-alpine' }
    }
    stages {
        stage('Restore') {
            steps {
                sh 'npm install'
            }
        }
        stage('Build') {
            steps {
                sh 'npm run-script build'
            }
        }
        stage('Test') {
            steps {
                sh 'ng run-script test'
            }
        }        
        stage('Deploy') {
            steps {
                sh 'rm ../../apps/*'
                sh 'cp ./dist/apps/* ../../apps/'
            }
        }             
    }
}
```
### 5. Set up Blue Ocean Pipeline
* Choose Platform by open blue ocean, by default the url is 
`localhost:8080/blue`
![](../img/Angular/choose_platform.png) you will see this


![](../img/Angular/connect-to-a-git-repository.png) you will see this

### Enjoy