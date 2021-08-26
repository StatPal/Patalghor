## 

#### Checkout one file from another branch

syntax:
`git checkout branch_name -- file_name
`

e.g.:
`
git checkout master               # first get back to master
git checkout dev -- app.js		  # then copy the version of app.js from branch "dev"
`

Or Update August 2019, Git 2.23
With the new git switch and git restore commands, that would be:

`
git switch master
git restore --source dev -- app.js
`
(source)[https://stackoverflow.com/q/2364147/16426739]


