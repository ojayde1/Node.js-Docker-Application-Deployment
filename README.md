# Node.js-Docker-Application-Deployment
# This project demonstrates how to create a simple Node.js application, containerize it using Docker, push the image to Docker Hub, and deploy it on an AWS EC2 instance.

---

## 🛠️ Tech Stack

- Node.js  
- Docker  
- AWS EC2  
- Docker Hub

---

## Application Description

This is a basic Node.js web application that displays the message:

> **WELCOME TO DATAFORTE!!**

---
## Create the Folder, change directory into it and create the files needed
mkdir <folder name>
touch <file name>

## 📁 Project Structure
├── app.js
├── Dockerfile
├── package.json
├── README.md

---

## ## Step 1: Create Node.js Application

Create a file named `app.js`, use vim command open the file and add the following:

const http = require('http');
const PORT = 5000;

--------

const server = http.createServer((req, res) => {
  res.writeHead(200, {'Content-Type': 'text/plain'});
  res.end('WELCOME TO DATAFORTE!!');
});

---
server.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`);
});

---

![Screenshot (98)](https://github.com/user-attachments/assets/26ffff66-30bd-4537-9f51-9622cac52b3a)

Then create a package.json file: using this command npm init -y
{
  "name": "dataforte-app",
  "version": "1.0.0",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  }
}

![Screenshot (100)](https://github.com/user-attachments/assets/a909b679-2924-449b-b8c8-320c4cdb9525)

install npm on the instance.

![Screenshot (93)](https://github.com/user-attachments/assets/0a776fb4-074d-43b4-ab78-2e50b2664108)

![Screenshot (94)](https://github.com/user-attachments/assets/ed728f57-014d-42a6-9315-c5eb06c33c27)

![Screenshot (95)](https://github.com/user-attachments/assets/1964f490-2c75-4b37-9e93-838ae62b6a87)

---

## Step 2: Write the Dockerfile
Create a file named Dockerfile and use vim to open it, add the following:

Dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package.json .
COPY app.js .

RUN npm install

EXPOSE 5000

CMD ["npm", "start"]

![Screenshot (99)](https://github.com/user-attachments/assets/bac9bea9-785f-4f88-945d-ba49c6847440)

---

## step 3: Deploy on AWS EC2
1. Launch an Ubuntu EC2 instance
Make sure the security group allows TCP traffic on port 5000.

2. Connect to the instance:
ssh -i your-key.pem ubuntu@your-ec2-public-ip

3. Install Docker:
sudo apt update
sudo apt install -y docker.io
sudo usermod -aG docker ubuntu
newgrp docker

---

## Step 4: Build Docker Image
Run the following command in your terminal to build the Docker image:
docker build -t node-docker-app .

![Screenshot (102)](https://github.com/user-attachments/assets/17898e37-a9bd-4097-a63a-8faa775432da)


---

## Step 5: Test the Docker Image Locally
Run the Docker container locally:

docker run -p 5000:5000 node-docker-app
Visit http://localhost:5000 to verify it shows:

![Screenshot (103)](https://github.com/user-attachments/assets/d5903649-87df-4654-ba40-3f8f5d198e8e)

WELCOME TO DATAFORTE!!

## Step 6: Push Image to Docker Hub
Login to Docker Hub

![Screenshot (104)](https://github.com/user-attachments/assets/0011b12e-fc00-4ff3-9bf8-1690346a6bf3)

If you created your account with Google, go to Docker Hub > Account Settings > Security to create a CLI password.
docker login -u <username>
Tag the Docker image:
docker tag node-docker-app ojayde35/node-docker-app:v1
Push to Docker Hub:
docker push ojayde35/node-docker-app:v1

![Screenshot (105)](https://github.com/user-attachments/assets/ed553335-7bfa-404b-97bc-78442d8fb017)


