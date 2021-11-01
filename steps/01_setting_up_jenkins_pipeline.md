# Setting up Jenkins Pipeline
1. Login to Jenkins and select "New Item"
2. Give an item name and select "Pipeline"
3. Select "Github project" and provide project path
4. Under "Build Triggers" select "Poll SCM" and set the Schedule to `* * * * *` to poll the GitHub every minute. Ensure you have GitHub plugin installed to have this option visible.
5. Under "Pipeline" select "Pipeline script from SCM" and select Git as your SCM
6. Generate a Personal Access Token (PAT) on GitHub and copy it.
7. Add the repository URL in the format `https://<PAT>@github.com/<REPO_URL>`
8. Ensure correct branch is specified in the "Branch" setting
9. Ensure correct script path is defined in the "Script Path" setting
10. Click on Save

[Home](../README.md)
