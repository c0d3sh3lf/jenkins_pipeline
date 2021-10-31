# Anchore
Anchore Engine is a system to help automate the description and enforcement of requirements on the content of docker containers.

With a bit more detail? Anchore Engine is a Docker container static analysis and policy-based compliance tool that automates the inspection, analysis, and evaluation of images against user-defined checks to allow high confidence in container deployments by ensuring workload content meets the required criteria. Anchore Engine ultimately provides a policy evaluation result for each image: pass/fail against policies defined by the user. Additionally, the way that policies are defined and evaluated allows the policy evaluation itself to double as an audit mechanism that allows point-in-time evaluations of specific image properties and content attributes.

## Downloading Anchore
1. To download Jenkins, please download the [Docker Compose](https://engine.anchore.io/docs/quickstart/docker-compose.yaml) file for Anchore and store it a directory
2. Edit and Save the file as per your requirements.
   ```
   networks:
    dockernet:
        external: true
   ```
   and add the below code under each service.
   ```
   networks:
    - dockernet
   ```
3. From the terminal, go to the directory and run `docker-compose up -d`
4. Your anchore engine should be up and running.

## Configuring Anchore to be used with Jenkins
1. Login to the Jenkins server and navigate to "Manage Jenkins" section and click on "Plugins Manager"
2. Search for "Anchore Container Image Scanner" and select it
3. Clcik on "Download and install after restart" and let the process compelete.
4. Go to "Manage Jenkins" and "Configure System". Scroll down till you find "Anchore Container Image Scanner"
5. Add the Anchore Url and the credentials and click on Save.

## Using Anchore in Jenkinsfile
You can add following code snippet in your Jenkinsfile for anchore processing
```
stage ('VA and Compliance') {
    steps {
        echo "Starting VA and Compliance for the $registry:latest image"
        sh 'echo "invad3rsam/flaskapp:latest `pwd`/Dockerfile" > anchore_images'
        anchore name: 'anchore_images', policyBundleId: 'anchore_cis_1.13.0_base', bailOnFail: false, bailOnPluginFail: false
        echo "VA and Compliance for $registry:latest image completed"
    }
}
```
[Home](../README.md)
