# git-housekeeping
 evacuated all project from server with comment "autosave"

## add all file  to the repos

```bash
 find . -maxdepth 1 -type d -print -execdir git --git-dir={}/.git --work-tree=$PWD/{} pull origin master \;
```

## pull all project in all sub folder

```bash
 find . -maxdepth 1 -type d -print -execdir git --git-dir={}/.git --work-tree=$PWD/{} pull origin master \;
```