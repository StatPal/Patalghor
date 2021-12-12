# Linux commands:


## Find command:

#### Find all files of a certain pattern:
Suppose, you want to list all the files with pattern `*.R`(, i.e., extension R), you can use:
```bash
find . -name "*.R"
```
If you want only the file names with the word `ggplot2` inside the file, you can use:
```bash
find . -name "*.R" -exec grep -l 'ggplot2' {} \;
```


#### Newer than Sep 10, Older than Sep 13
```bash
find . -type f -newermt "Sep 10" \! -newermt "Sep 13" -print
# or
# find . -type f -newermt "Sep 10" \! -newermt "Sep 13" -exec echo {} \;
```
#### To delete those:
```bash
find . -type f -newermt "Sep 10" \! -newermt "Sep 13" -delete
# or
# find . -type f -newermt "Sep 10" \! -newermt "Sep 13" -exec rm {} \;
```

### If you want to search and replace all the occurances:
```bash
find . -name '*<pattern-of-the-files>' -exec sed -i -e 's/<search-pattern>/<replace-pattern>/g' {} \;
```

## Batch copy rename:
```bash
lis=($(ls <pattern>))
for file in "${lis[@]}"
do
  cp -i "$file" "${file/<original-pattern>/<target-pattern>}"
  echo item: $file
done
```

#### Remove leading whitespaces:
```bash
sed -e 's/^[ \t]*//'
```
[source](https://stackoverflow.com/questions/369758/how-to-trim-whitespace-from-a-bash-variable)


#### tar and zips
```bash
tar -czvf name-of-archive.tar.gz /path/to/directory-or-file
```
The meaning:
-c: Create an archive.
-z: Compress the archive with gzip.
-v: Display progress in the terminal while creating the
-f: Specify the file name.



#### Tricks:

##### nano tricks: (Let's admit, nano is a good enough editor to do short jobs)

To insert some output from some command-ine output,
 - Do `^R` (To read 'something')
 - Do `^X` (To execute commnd)
 - Type command to execute, like `date`, then enter.


To comment many lines at once:
 - Select the lines using Shift+arrow keys/mouse
 - `Esc + 3`
 - https://unix.stackexchange.com/questions/460474/how-to-comment-multiple-lines-in-nano-at-once

 Similarly, to indent, you have to use `Alt+}` and other things,  https://unix.stackexchange.com/a/639082/481190
 and https://stackoverflow.com/questions/7170103/keyboard-shortcut-to-comment-a-line-in-nano
