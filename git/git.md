##

Git is a "free ... distributed version control system ... to handle ... small to very large projects with speed and efficiency."

Basically, you don't have to rename your poject as "project(copy)(new)final-2.tex" etc etc etc. You can handle these kinds of stuff more easily. 

Moreover, many people can add soemthing into a project in an easy way. 


## Main workflow of git:

- To update your local repository 
```bash 
git pull
```
- To add something/to stage changes for commiting: 
```bash 
git add <filename>
```
-To commit changes: 
```bash
git commit -m "message"
```
- To push these changes to the remote repository:
```bash
git push
```

## Terminology:
 - *local* 
 - *remote repo* -- If you store your project into some online repository (like github/gitlab/bitbucket) privately or publicly - this is where you store it. 
 - *branch* -- Your git repo(sitory) can have morre than one branch - maybe one for final product, maybe one for development type 1, one for development type 2 etc. You can copy/merge one branche('s) file to another. So you can experiment in *dev branch*, then merge it to the *master* branch.
 







### Git diff:


To get the diff for the Staged file (after git add . or equaivalent)
```bash
git diff --staged
```



#### diff among branches:
```bash
git diff branch_1...branch_2
```

Get differences of two specific files:
```bash
git diff branch_1..branch_2 -- path/to/myfile
# or
git diff branch_1 branch_2 -- path/to/myfile
```

Only to know which files differ:
```bash
git diff --name-only branch_1...branch_2
```

Compare checked out branch to branch_2
```bash
git diff ..branch_2
```
[source](https://stackoverflow.com/q/9834689/16426739)





### Checkout one file from another branch

syntax:
```bash
git checkout branch_name -- file_name
```
e.g.:
```bash
git checkout master               # first get back to master
git checkout dev -- app.js	  # then copy the version of app.js from branch "dev"
```
Or Update August 2019, Git 2.23
With the new git switch and git restore commands, that would be:
```bash
git switch master
git restore --source dev -- app.js
```
[source](https://stackoverflow.com/q/2364147/16426739)


