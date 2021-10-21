# SonarQube

## Downloading and Configuring SonarQube with Jenkins
* Run the below commands
```
docker pull sonarqube
docker run -d --name sonarqube -p 9000:9000 --net=dockernet -v sonarqube_data:/opt/sonarqube/data -v sonarqube_extensions:/opt/sonarqube/extensions -v sonarqube_logs:/opt/sonarqube/logs --stop-timeout 3600 sonarqube 
```
* Configure SonarQube. Default administrator username and password for SonarQube would be `admin:admin`.
* Update the password at first login


## Adding code to Jenkinsfile
Add below code in the Jenkinsfile
```
environment {
        registry = "invad3rsam/flaskapp"
        registryCredentials = "DockerHub"
        dockerImage = ''
        scannerHome = tool 'SonarQube Scanner'
    }
```
And then create a Code Review stage with the below code
```
stage ("Code Review"){
            /* This stage will perform code review of the code and sleep for 5 seconds to complete the code analysis and publish the results */
            steps{
                echo "Starting code review"
                script {
                    withSonarQubeEnv() {
                        sh "$scannerHome/bin/sonar-scanner \
                        -Dsonar.projectKey=Jenkins_sample_flask \
                        -Dsonar.host.url=http://192.168.4.2:9000 \
                        -Dsonar.login=6d5ce1d9fe527e2feb9a21b3f1b705e1ab213e48 \
                        -Dsonar.sources=/var/jenkins_home/workspace/Git_Flask_Pipeline \
                        -Dsonar.scm.provider=git"
                    }
                    sleep(30)
                    timeout(time: 1, unit:'MINUTES') {
                        def qg = waitForQualityGate()
                        if (qg.status != 'OK') {
                            error "Pipeline aborted due to quality gate failure: ${qg.status}"
                        }
                    }
                }
                echo "Code Review Completed"
            }
        }
```

[Home](../README.md)
