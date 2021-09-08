# Linux commands:


## Find command:

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


#### Tricks:

##### nano tricks: (Let's admit, nano is a good enough editor to do short jobs)

To insert some output from some command-ine output,
 - Do `^R` (To read 'something')
 - Do `^X` (To execute commnd)
 - Type command to execute, like `date`, then enter.
