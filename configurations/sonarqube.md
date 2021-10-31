# SonarQube

## Downloading and Configuring SonarQube with Jenkins
* Run the below commands
```
docker pull sonarqube
docker run -d --name sonarqube -p 9000:9000 --net=dockernet -v sonarqube_data:/opt/sonarqube/data -v sonarqube_extensions:/opt/sonarqube/extensions -v sonarqube_logs:/opt/sonarqube/logs --stop-timeout 3600 sonarqube 
```
* Configure SonarQube. Default administrator username and password for SonarQube would be `admin:admin`.
* Update the password at first login

## Adding Sonarqube plugin
1. Login to Jenkins and go to "Manage Jenkins" and select "Plugin Manager"
2. Look for "SonarQube Scanner for Jenkins" plugin and select it
3. Select "Download and install after Restart"

## Configuring Sonarqube plugin
### SonarQube
1. Login into Sonarqube
2. Create a project (Manually) and make a note of the Display name and the project key that you provide
3. Then next page, select Locally and then generate a token and copy the token as you will not be able to see this token later.

### Jenkins
1. Login into Jenkins and navigate to "Manage Jenkins" and select "Manage Credentials"
2. Select the Jenkins store and add a new credential
3. Select "Secret Text" in the dropdown and then paste your token here. Provide an ID and description for the token to be used later.
4. Save the credentials.
5. Now, go to "Manage Jenkins" and "Configure System"
6. Scroll down till you find SonarQube
7. Ensure "nvironment variables Enable injection of SonarQube server configuration as build environment variables" is enabled
8. Add a name to your SonarQube server
9. Provide the server URL and Provide the key that you have saved in Step No. 4
10. Click on Apply and Save.
11. Now, we need to install SonarScanner. To do that go to "Manage Jenkins" and then select "Global Tool Configuration".
12. Scroll Down to "SonarQube Scanner"
13. Add a new Scanner and provide a name to the scanner.
14. Select the version of the Sonarscanner to be installed
15. Click on Apply and Save.


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
                        -Dsonar.projectKey=Jenkins_sample_flask \       \\ Value of Project Key created in SonarQube
                        -Dsonar.host.url=http://192.168.4.2:9000 \      
                        -Dsonar.login=6d5ce1d9fe527e2feb9a21b3f1b705e1ab213e48 \        \\ Value of token created in the SonarQube
                        -Dsonar.sources=/var/jenkins_home/workspace/Git_Flask_Pipeline \        \\Change the last value to the Pipeline name
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
