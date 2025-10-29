1. created a github account<br>
    .https://github.com<br>
2. Installed git on local system(windows)<br>
3. verified the installation of git<br>
    .git<br>
    .git --version<br>
4. Connect github to git<br>
    .git config --global user.name"signed_in_username"<br> 
    .git config --global user.email"signed_in_email"<br>
    .git config --global list #To verify<br>
5. create a directory and change into the directory<br>
    .mkdir Director_name <br>
    .cd Director_name<br>
6. Initialize git<br>
    .git init<br>
7. Create Documentation file<br>
    .touch file_name<br>
8. Enter into the created documentation file(in VS code)<br>
    .code file_name<br>
9. check git status and add the file<br>
    .git status<br>
    .git add file_name<br>
10. Commit the changes<br>
    .git commit -m "text"<br>
11. Create a repository in github<br>
    .open the github account<br>
    .click on Repositories<br>
    .click on new<br>
    .create a new repo<br>
12. link local repo to git and push the file<br>
    .git remote add origin https://github.com/<user.name>/<repo_name>.git<br>
    .git branch -M main<br>
    .git push -u origin main<br>
13. Now youll have the file within the Repo in the github.<br>
       DONE