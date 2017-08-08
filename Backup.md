# GitBucket backup

The following page describes an example of a possible [backup script](https://github.com/takezoe/gitbucket/wiki/Backup#backup-script) for your GitBucket installation.

Feel free to inspire from it.

## What it does?

The backup of a GitBucket installation should be consistent between the database state and the state of the git repositories.

Of course the most important to keep is probably the git repositories, but hey if you need sometime to recover it would be cool that all PRs, issues, references and so on are in synch with the backups of the repositories no?

The provided [backup script](https://github.com/takezoe/gitbucket/wiki/Backup#backup-script) tries to minimise as much as possible the time between the database backup and a clean stage of all repositories by doing the following steps:

- clone all repositories into a backup folder (creating only non already existing ones)
- update all repositories
- make a database backup
- update again all repositories

## Using backup.sh

How to use the provided [backup script](https://github.com/takezoe/gitbucket/wiki/Backup#backup-script)?

`bash backup.sh [-v] GITBUCKET_HOME BACKUP_FOLDER [Database backup URL]`

where

- -v: stands for verbose, when used the script will output more about what it is doing. Optional
- GITBUCKET_HOME: is the full path to your GitBucket data folder. By default it might be `~/.gitbucket`
- BACKUP_FOLDER: is the full path to the folder into which you would like the backup to be done
- Database backup URL: it is the full URL that forces GitBucket installation to perform a database export (see [PR-845](https://github.com/takezoe/gitbucket/pull/845)). This parameter is optional ; thus if it is omitted then no database export will be done.

## Using from Windows

I tested this script on windows using a msysgit installation (one copy of one coming bundled with [Sourcetree git client](https://www.sourcetreeapp.com/)).

Here is a `backup.bat` file that could be launched on the server hosting GitBucket

```bat
@echo off

REM Go to script directory
CD /D %~dp0

REM Add all msyggit commands in the path (bash, git,
SET PATH=D:\tools\portable-git-1.9.5\bin;%PATH%

bash backup.sh d:\gitbucket\work e:\backup\gitbucket http://localhost:8080/database/backup
```

## Backup script

This is an example of backup script

```bash
#!/bin/bash

DEBUG=0
if [ "$1" = "-v" ]
then
    DEBUG=1
    shift
fi

GITBUCKET_DATA=$1
BACKUP_FOLDER=$2
GITBUCKET_DB_BACKUP_URL=$3

gitbucketRepositoriesFolder=$GITBUCKET_DATA/repositories
repositoriesFolderNameSize=${#gitbucketRepositoriesFolder}

repositoriesBackupFolder=$BACKUP_FOLDER/repositories

##
## trace allows to print messages on the console if -v argument (verbose) has been given to the program
## 
function trace {
    if [ "$DEBUG" = "1" ]
    then
        echo "$@"
    fi
}

##
## create a git mirror clone of a repository. If the clone already exists, the operation is skipped
##    arg1: source of the repository to be cloned
##    arg2: full folder name of the clone
## 
function createClone {
    source=$1
    dest=$2
    
    if [ ! -d $dest ]
    then
        trace "  cloning $source into $dest"
        git clone --mirror $source $dest > /dev/null 2>&1
    else
        trace "  $dest already exists, skipping git clone operation"
    fi
}

##
## update a clone folder, the update is down toward the latest state of its default remote
##
function updateRepository {
    currentFolder=$(pwd)
    cd $1
    trace "  updating $1"
    git remote update > /dev/null
    cd $currentFolder
}

# Perform some initializations
if [ ! -d $repositoriesBackupFolder ] 
then
    mkdir -p $repositoriesBackupFolder > /dev/null
fi


#
# To keep integrity as its maximum possible, the database export and git backups must be done in the shortest possible timeslot.
# Thus we will:
#   - clone new repositories into backup directory
#   - update them all a first time, so that we gather gib updates
#   - export the database
#   - update all repositories a second time, this time it should be ultra-fast
#

# First let's be sure that all existing directories have a clone
# as clone operation can be heavy let's do it now
repositories=$(find $gitbucketRepositoriesFolder -name *.git -print)

echo "Starting clone process"
for fullRepositoryFolderPath in $repositories
do
    repositoryFolder=${fullRepositoryFolderPath:$repositoriesFolderNameSize}
    mirrorFolder=$repositoriesBackupFolder$repositoryFolder

    createClone $fullRepositoryFolderPath $mirrorFolder
done;
echo "All repositories, cloned"

echo "Update repositories: phase 1"
#
# Then let's update all our clones
# 
for fullRepositoryFolderPath in $repositories
do
    repositoryFolder=${fullRepositoryFolderPath:$repositoriesFolderNameSize}
    mirrorFolder=$repositoriesBackupFolder$repositoryFolder

    updateRepository $mirrorFolder
done;
echo "Update repositories: phase 1, terminated"

#
# Export the database
# 
if [ "$GITBUCKET_DB_BACKUP_URL" != "" ]
then
    echo "Database backup"
    curl $GITBUCKET_DB_BACKUP_URL > /dev/null
    cp -f $GITBUCKET_DATA/gitbucket-database-backup.zip $BACKUP_FOLDER > /dev/null
else
    echo "No database URL provided, skipping database backup"
fi

#
# Export the GitBucket configuration
# 
echo "Configuration backup"
cp $GITBUCKET_DATA/gitbucket.conf $BACKUP_FOLDER > /dev/null

#
# Export the GitBucket data directory (avatars, ...)
# 
echo "Avatars backup"
tar -cf $BACKUP_FOLDER/data.tar $GITBUCKET_DATA/data > /dev/null 2>&1
gzip -f $BACKUP_FOLDER/data.tar > /dev/null

#
# Then let's do a final update
# 
echo "Update repositories: phase 2"
for fullRepositoryFolderPath in $repositories
do
    repositoryFolder=${fullRepositoryFolderPath:$repositoriesFolderNameSize}
    mirrorFolder=$repositoriesBackupFolder$repositoryFolder

    updateRepository $mirrorFolder
done;
echo "Update repositories: phase 2, terminated"

echo "Update process ended, backup available under: $BACKUP_FOLDER"
```