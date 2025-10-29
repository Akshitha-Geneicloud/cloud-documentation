1.created a github account
    .https://github.com
2.Installed git on local system(windows)
3.verified the installation of git
    .git
    .git --version
4.Connect github to git
    .git config --global user.name"signed_in_username" 
    .git config --global user.email"signed_in_email"
    .git config --global list #To verify
5.create a directory and change into the directory
    .mkdir Director_name 
    .cd Director_name
6.Initialize git
    .git init
7.Create Documentation file
    .touch file_name
8.Enter into the created documentation file(in VS code)
    .code file_name
9.check git status and add the file
    .git status
    .git add file_name
10.Commit the changes
    .git commit -m "text"
11.Create a repository in github
    .open the github account
    .click on Repositories
    .click on new
    .create a new repo
12.link local repo to git and push the file
    .git remote add origin https://github.com/<username>/<repo_name>.git
    .git branch -M main
    .git push -u origin main
13.Now youll have the file within the Repo in the github.