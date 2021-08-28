## Linux commands:


## Find command:

#### Newer than Sep 10, Older than Sep 13
```
find . -type f -newermt "Sep 10" \! -newermt "Sep 13" -print
# or
# find . -type f -newermt "Sep 10" \! -newermt "Sep 13" -exec echo {} \;
```
#### To delete those:
```
find . -type f -newermt "Sep 10" \! -newermt "Sep 13" -delete
# or
# find . -type f -newermt "Sep 10" \! -newermt "Sep 13" -exec rm {} \;
```

### If you want to search and replace all the occurances:
```
find . -name '*<pattern-of-the-files>' -exec sed -i -e 's/<search-pattern>/<replace-pattern>/g' {} \;
```


### Batch copy rename:
```
lis=($(ls <pattern>))
for file in "${lis[@]}"
do
  cp -i "$file" "${file/<original-pattern>/<target-pattern>}"
  echo item: $file
done
```


