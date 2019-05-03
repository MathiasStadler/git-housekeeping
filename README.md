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

alias git-housekeeping= "f() {
echo "=> git-housekeeping"
if [[ \$# -eq 0 ]];
then
echo 'what will you do';
exit 1;
fi;
echo "param $1";
}; f;"
EOF

chmod +x $SCRIPT_NAME

#load method in bash
source ./$SCRIPT_NAME


```


