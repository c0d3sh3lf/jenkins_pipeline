# Embedding GitHub Build Status

Embedding GitHub Build Status in the GitHub Repository.

## Steps:
1. Login into Jenkins and go to "Manage Jenkins". Select "Manage Plugins".
2. Search for "Embeddable Build Status" in Available plugins and install it.
3. You can add below sample code in your pipeline code and modify as per your needs.
```
def win32BuildBadge = addEmbeddableBadgeConfiguration(id: "win32build", subject: "Windows Build")

pipeline {
    agent any
    stages {
        steps {
            script {
                win32BuildBadge.setStatus('running')
                try {
                    RunBuild()
                    win32BuildBadge.setStatus('passing')
                } catch (Exception err) {
                    win32BuildBadge.setStatus('failing')

                    /* Note: If you do not set the color
                             the configuration uses the best status-matching color.
                             passing -> brightgreen
                             failing -> red 
                             ...
                    */
                    win32BuildBadge.setColor('pink')

                    error 'Build failed'
                }
            }
        }
    }
}
```

Ref: [Embeddable Build Status|Jenkins Plugin](https://plugins.jenkins.io/embeddable-build-status/)
[Home](../README.md)
