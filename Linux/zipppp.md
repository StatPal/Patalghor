# Basic individual commands:

#### Simple zip
```bash
zip myfile.zip filename.txt
unzip myfile.zip
```
#### Simple gzip
```bash
gzip filename.txt      ## becomes filename.txt.gz
gunzip filename.txt.gz
```
#### Simple tar
When you have many files(e.g., in a folder), you would use `tar` to make it one file and gzip to zip it. Often they are done at the same time. 


## Zip and tar/Unzip and untar - all in one. 

Often, a folder is composed into a file first using `tar` and then gzipped, with the file extension `*.tar.gz`. To unzip and untar such files, use: 
```bash
tar -xzf something.tar.gz 
```
Here `-x` means `extract`, `z` means `gz` type zipping, `f` means `file name`. Add `-C /target/dir` for target directory or `-v` for `verbose` if necessary. 
**If you have `tar.bz2` file, use `-j` instead of `-z` and if you have `tar.xz` file, use `-J` instead of `-z`.**

-------------------------------------------------------------------------
If you want to create a tar.gz type file:
```bash
tar -czf something.tar.gz path/to/file/or/directory
```
Here `-c` means `create`, `z` means `gz` type zipping, `f` means `file name`. (Add `-v` to `verbose`)

-------------------------------------------------------------------------
If you want to know what's in that archive file:
```bash
tar -tvf something.tar.gz
```
See https://www.cherryservers.com/blog/how-to-archive-and-compress-files-with-the-tar-and-gizp-commands-in-linux
