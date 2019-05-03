# git-housekeeping
 evacuated all project from server with comment "autosave"

## add all file  to the repos

```bash
find . -maxdepth 1 -type d -print -execdir git --git-dir={}/.git --work-tree=$PWD/{} add . \;
```

## commit all projects

```bash
find . -maxdepth 1 -type d -print -execdir git --git-dir={}/.git --work-tree=$PWD/{} commit -am "auto save" \;
```

## push all projects

```bash
find . -maxdepth 1 -type d -print -execdir git --git-dir={}/.git --work-tree=$PWD/{} push \;
```

## pull all project in all sub folder

```bash
find . -maxdepth 1 -type d -print -execdir git --git-dir={}/.git --work-tree=$PWD/{} pull origin master \;
```

- or

```bash
find . -name .git -type d -prune | while read d;
do cd $d/..;
echo "$PWD >" git pull;
git pull;
cd $OLDPWD;
done
```

- or only for a special github account

```bash
find . -name .git -type d -prune | while read d;
do cd $d/..;
echo "$PWD >" git pull;
repoURL=$(git config --get remote.origin.url)
gitHubAccountName="MathiasStadler"
echo "repo URL => "$repoURL;
repoURL=$(git config --get remote.origin.url)
gitHubAccountName="MathiasStadler"
if echo "$repoURL" | grep -Eq  "$gitHubAccountName";
then
echo "repo of $gitHubAccountName"
git pull
else
echo "repo of another"
fi;
cd $OLDPWD;
done
```

## bash function

```bash
SCRIPT_NAME="git-housekeeping.sh"
cat << EOF >$SCRIPT_NAME
#!/bin/bash

alias githousekeeping="f(){ \\
echo => git housekeeping; \\
if [[ \\\$# -eq 0 ]]; \\
then \\
echo what will you do; \\
else \\
echo found action \\\$1 ; \\
    find . -name .git -type d -prune | while read d;
    do \\
    cd \\\$d/..; \\
    echo current directory \\\$PWD; \\
    if [ \\\$1 == "check" ]; \\
    then \\
        echo action check; \\
    elif [ \\\$1 == "push" ]; \\
    then \\
        echo action push; \\
    elif [ \\\$1 == "pull" ]; \\
    then \\
        echo \\\$d action pull; \\
        echo git --git-dir=\\\$d/.git --work-tree=\\\$d pull origin master
    elif [ \\\$1 == "add" ]; \\
    then \\
        echo action add; \\
    elif [ \\\$1 == "commit" ]; \\
    then \\
        echo action commit; \\
    else \\
        echo action not found; \\
    fi; \\
    cd \\\$OLDPWD;
    done
fi; \\
}; f"
EOF

chmod +x $SCRIPT_NAME

#load method in bash
source ./$SCRIPT_NAME


```

```bash
alias githouse='f() { if [[ $# -eq 0 ]]; then echo what will you do; else echo $1 ;fi; }; f'
```