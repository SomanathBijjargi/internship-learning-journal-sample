# Week 1 , Session 4 and 5
## Proper installation of git in WSL 

**Step 1:** Initialization of git 
```
git init
```
**Step 2:** Setup Configration
```
git config --global user.name <Your Name>
git config --global user.email <youremail@example.com>
```
**Step 3:** Adding the remote repository 
```
git remote add origin <REMOTE_URL>
```
**Step 4:** to put the files to staging 
```
git add <filename>
```
OR to add all the files to stage
```
git add .
```

**Step 5:** Commit the changes with commit message
```
git commit -m "Commit Message"  
```

**Step 6:** Push the local files to remote Repository
```
git push origin main
```

here "**main**" is the branch