# SonarQube

## Downloading and Configuring SonarQube with Jenkins
* Run the below commands
```
docker pull sonarqube
docker run -d --name sonarqube -p 9000:9000 --net=dockernet -v sonarqube_data:/opt/sonarqube/data -v sonarqube_extensions:/opt/sonarqube/extensions -v sonarqube_logs:/opt/sonarqube/logs --stop-timeout 3600 sonarqube 
```
* Configure SonarQube. Default administrator username and password for SonarQube would be `admin:admin`.
* Update the password at first login
