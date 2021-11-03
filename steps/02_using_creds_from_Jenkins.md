# Using credentials stored in Jenkins Credential Manager

Store the credentials first
1. Login into Jenkins and select "Manage Jenkins" from left menu.
2. Goto "Manage Credentials" and select the store (default store is "Jenkins")
3. Select the domain (default is "Global credentials (unrestricted)")
4. Select add credentials and select apropriate type of credentials.
5. Ensure to note down the ID

# Using username and password credentials
Add the below snippet of code to the stage where you need to retrieve the credentials. Credential variables can only be used within the body of `withCredentials` function.
```
  withCredentials([usernameColonPassword(credentialsId: '<ID_FROM_STEP_5>', variable: 'USERPASS')]){
    // use credentials with variable name as USERPASS here
  }
```

```
  withCredentials([usernamePassword(credentialsId: '<ID_FROM_STEP_5>', passwordVariable: 'password', usernameVariable: 'username')]){
    // use credentials as username and password variables
  }
```

# Using secret text credentials
```
  withCredentials([string(credentialsId: '<ID_FROM_STEP_5>', variable: 'secret')]){
    // use credentials with variable name as secret here
  }
```

[Using credentials in scripted Jenkins pipelines – Schneide Blog](https://schneide.blog/2021/06/02/using-credentials-in-scripted-jenkins-pipelines/)

[Credentials Binding Plugin](https://www.jenkins.io/doc/pipeline/steps/credentials-binding/#withcredentials-bind-credentials-to-variables)

[Home](../README.md)
