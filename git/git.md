# Git

- [Main commands of git](#main-commands-of-git)
    + [(One-time) Configure/Setup](#-one-time--configure-setup)
  * [Main workflow of git](#main-workflow-of-git)
    + [Another two common functions:](#another-two-common-functions)
- [Terminologies](#terminologies)
- [Specific commands](#specific-commands)
    + [Git add or deleting](#git-add-or-deleting)
    + [Git diff](#git-diff)
    + [diff among branches](#diff-among-branches)
    + [Checkout one file from another branch](#checkout-one-file-from-another-branch)
    + [Git log](#git-log)
    + [Remotes](#remotes)
    + [Tags](#git-tag)

**Why you might use git??**

 - **Basically, one doesn't have to rename poject many times as "project(copy)(new)final-2.tex" etc etc etc. Git makes these much much easier to handle**. 
 - Moreover, many people can add soemthing into a project in an easy way.*
 - You can show/publish your code to the rest of the world. 
 - Help pages/bio can also be published.

Technically, Git is a "free ... distributed version control system ... to handle ... small to very large projects with speed and efficiency."



## Main commands of git

#### (One-time) Configure/Setup
```bash
git config --global user.name "YOUR NAME"
git config --global user.email "YOUR EMAIL ADDRESS"
git config --global push.default simple
```
For authentication, it is easiest to use `ssh` or `secure shell` [See this](https://docs.github.com/en/github/authenticating-to-github/connecting-to-github-with-ssh)

To initialize a git repository(=repo), you may do:
```bash
git init <directory>
```

To clone some existing repo (in say `github`), you can do something like:
```bash
git clone git@github.com:<username>/<repo>.git
```


### Main workflow of git
- To update your local repository 
```bash 
git pull
```
- To add something/to stage changes for commiting: 
```bash 
git add <filename>
```
- To commit changes: 
```bash
git commit -m "your message"
```
- To push these changes to the remote repository:
```bash
git push
```

#### Another two common functions
To check the current changes after adding a file etc, use:
```bash
git status
```
To see the exact differences in the file(s):
```bash
git diff
```

## Terminologies
 - *local repository* -- repository in your local machine.
 - *remote/upstream repo(sitory)* -- If you store your project into some online repository (like github/gitlab/bitbucket) privately or publicly - this is where you store it. 
 - *branch* -- Your git repo(sitory) can have morre than one branch - maybe one for final product, maybe one for development type 1, one for development type 2 etc. You can copy/merge one branche('s) file to another. So you can experiment in *dev branch*, then merge it to the *master* branch.
 - *commit* (as verb) -- 'The action of storing a new snapshot of the project’s state in the Git history, by creating a new commit representing the current state of the index and advancing HEAD to point at the new commit.'
 - *commit* (as noun) -- A single point in the Git history; the entire history of a project is represented as a set of interrelated commits. Synonymous to "revision" or "version."
 - *Tracked and untracked files* - files either in the index cache or not yet added to it
 - *Cache* - a space intended to temporarily store uncommitted changes
 - *Stash* - another cache, that acts as a stack, where changes can be stored without committing them



## Specific commands

#### Git add or deleting

To add two files and/or whole directory at once,
```bash
git add <filename_1> <filename_2> <directory_1> 
```
To add all the files under the current directories, 
```bash
git add .
```
To remove a file/directory, use similar coommands like
```bash
git rm <filename>
```



#### Git diff

To get the diff for the Staged file (after git add . or equaivalent)
```bash
git diff --staged
```



#### diff among branches
```bash
git diff branch_1...branch_2
```

Get differences of two specific files:
```bash
git diff branch_1..branch_2 -- path/to/myfile
# or
git diff branch_1 branch_2 -- path/to/myfile
```

Only to know which files differ
```bash
git diff --name-only branch_1...branch_2
```

Compare checked out branch to branch_2 [source](https://stackoverflow.com/q/9834689/16426739): 
```bash
git diff ..branch_2
```


#### Checkout one file from another branch

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

#### git log
```bash
git log
```
show recent commits, most recent on top. Useful options: --color with color --graph with an ASCII-art commit graph on the left --decorate with branch and tag names on appropriate commits --stat with stats (files changed, insertions, and deletions) -p with full diffs --author=foo only by a certain author --after="MMM DD YYYY" ex. ("Jun 20 2008") only commits after a certain date --before="MMM DD YYYY" only commits that occur before a certain date --merge only the commits involved in the current merge conflicts. [source](https://gist.github.com/iansheridan/870778)


#### Remotes


#### git tag
You can give tags to some commits which you think are important, like:
```bash
git tag <tag-name>
```
Then push it to remote repository by:
```bash
git push origin <tag-name>
```
If you want to push all tags, you may try `git push --tags`


To remove a tag:
```bash
git tag -d <tag-name>
```
and to push that to remote
```bash
git push --delete origin <tag-name>
```



