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
GITHUB_ACCOUNT_URL=\"\\\${HOME}/.gitHubAccountURL.info\"; \\
echo => git housekeeping; \\
if [[ \\\$# -eq 0 ]]; \\
then \\
echo what will you do? check commit push pull repoIdentifier findRepository ; \\
elif [ \\\$1 == "repoIdentifier" ]; \\
    then \\
        echo found action \\\$1 ; \\
        echo action repoIdentifier; \\
        read -p \"enter unique identifier for repository that contain in the remote URL e.g. account name => \" repoIdentifier ; \\
        echo \\\$repoIdentifier >\\\${GITHUB_ACCOUNT_URL}; \\
        printf \"set GitHub Account URL to => %s \" \"\\\$(cat \\\${GITHUB_ACCOUNT_URL})\" ; \\
else \\
echo found action \\\$1 ; \\
    find . -name .git -type d -prune | while read d;
    do \\
    cd \\\$d/..; \\
    echo current directory \\\$PWD; \\
    if [ \\\$1 == "check" ]; \\
    then \\
        echo action check; \\
        git status --porcelain | awk '/^\\\?\\\?/ { print \\\$2; }'; \\
        UPSTREAM=\\\${1:-@{u}}; \\
        LOCAL=\\\$(git rev-parse @); \\
        REMOTE=\\\$(git rev-parse \"\$UPSTREAM\"); \\
        BASE=\\\$(git merge-base @ \"\$UPSTREAM\"); \\
        if [ \\\$LOCAL = \\\$REMOTE ]; then \\
            echo  \\\$PWD Up-to-date; \\
        elif [ \\\$LOCAL = \\\$BASE ]; then \\
            echo  \\\$PWD Need to pull; \\
        elif [ \\\$REMOTE = \\\$BASE ]; then \\
            echo  \\\$PWD Need to push; \\
        else \\
            echo  \\\$PWD Diverged; \\
        fi; \\
    elif [ \\\$1 == "push" ]; \\
    then \\
        echo action push; \\
         URL=\$(git config --get remote.origin.url); \\
        DIRNAME=\$(dirname \$URL ); \\
        echo URL => \$URL; \\
        echo DIRNAME => \$DIRNAME; \\
    elif [ \\\$1 == "pull" ]; \\
    then \\
        echo \\\$d action pull; \\
        git pull origin master; \\
    elif [ \\\$1 == "add" ]; \\
    then \\
        echo action add; \\
        git add .;\\
    elif [ \\\$1 == "commit" ]; \\
    then \\
        echo action commit; \\
        git commit -am \"auto save\"
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


cd ./git-housekeeping; \\
        dirname \\\$(git config --get remote.origin.url) >\\\${GITHUB_ACCOUNT_URL}; \\


 if [\"\\\$(dirname \\\$(git config --get remote.origin.url) >\\\${GITHUB_ACCOUNT_URL})\" =~ \"\\\$(cat \\\${GITHUB_ACCOUNT_URL})\" ] ; then \\
                echo "ok"; \\
                git push; \\
            else \\
                echo "not ok"; \\
            fi; \\