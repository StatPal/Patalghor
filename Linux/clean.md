# Clean your machine:

## Clean everything which are **usually** not needed for your linux (or manybe *nix) machine:


##### Check and clean Journal files (just delete everything more than 100MB):
```
journalctl --disk-usage
journalctl --vacuum-size=100M
```

##### Check and clean coredump files(if you don't want to check the coredump files):
```
ls -lht /var/lib/systemd/coredump/
rm /var/lib/systemd/coredump/*
```

#### Find the cache files which are more than 30 days old and delete them (these are inside $HOME/.cache folder)
```
# find ~/.cache/ -type f -mtime +30 -print  			## if you want to see those.
find ~/.cache/ -type f -mtime +30 -exec rm -rf {} \;	## delete those
```


## Show current disk usage:
```
echo "\n\nCurrent disk usage:\n"
df -h
du --all -h -d 2 --threshold=50M
```


