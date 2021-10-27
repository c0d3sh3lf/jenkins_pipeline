# Setting up Jenkins Pipeline via Docker

## Downloading and configuring image
* Run the below commands in terminal
```
docker pull jenkinsci/blueocean
docker run -p 8080:8080 -p 50000:50000 -v jenkins:/var/jenkins_home -v /var/run/docker.sock:/var/run/docker.sock --name jenkins --net=dockernet jenkinsci/blueocean
```
* You will find an administrators password in the terminal. Copy the password and browse [http://localhost:8080](http://localhost:8080) and paste the administrators password when asked.
* Install the default set of plugins by selecting "Install recommended plugins" or similar setting.
* Create your user and login with your user.
* Update any plugin that needs update.
* Stop and remove the container. Your jenkins is now ready to server the pipelines.

Below are few integrations for Jenkins
* [Anchore](configurations/anchore.md)
* [SonarQube](configurations/sonarqube.md)
