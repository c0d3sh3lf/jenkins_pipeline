# Setting up Jenkins Pipeline via Docker

## Step 1: Downloading and configuring images

docker pull jenkinsci/blueocean

docker run -p 8080:8080 -p 50000:50000 -v jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock --name jenkins --net=dockernet jenkinsci/blueocean
