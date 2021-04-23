# Git Cheatsheet

## Initial Config

### General

#### Change default branch name

```sh
git config --global init.defaultBranch <DEFAULT_NAME>
```

#### Change default editor

```sh
git config --global core.editor nano
```

#### Change user name

```sh
git config --global user.name "<FIRST_NAME> <LAST_NAME>"
```

#### Change user email

```sh
git config --global user.email "<EMAIL_ADRESS>"
```

### Aliases

```sh
git config --global alias.co checkout
git config --global alias.br 'branch -av'
git config --global alias.st status
git config --global alias.unstage 'reset HEAD --'
git config --global alias.a add
git config --global alias.amend 'commit --amend'
git config --global alias.ci commit
git config --global alias.cm 'commit -m'
git config --global alias.last 'log -l HEAD'
git config --global alias.visual '!gitk'
```

### Remote

#### Show all remote

```sh
git remote -v
```

#### Add a new remote

```sh
git remote add <LOCAL_ID> <URL>
```

#### Update a remote

```sh
git remote set-url <LOCAL_ID> <URL>
```

## Repository

### Initialising

#### Create a new repository

```sh
git init
```

#### Cloning a repository

##### Deep clone

```sh
git clone <URL>
```

##### Shallow clone

```sh
git clone --depth <NUMBER_OF_COMMIT_FROM_THE_HEAD> <URL>
```

## Branch

### Branch

#### List all existing branches with the latest commit comment

```sh
git branch -av
```

#### Create a branch

```sh
git branch <NEW_BRANCH>
```

#### Delete a local branch

```sh
git branch -d <BRANCH>
```

#### Force the deletion of a local branch

```sh
git branch -D <BRANCH>
```

### Checkout

####  Switch HEAD to branch

```sh
git checkout <BRANCH>
```

#### Switch HEAD to previous branch

```sh
git checkout -
```

#### Create a branch (checkout version)

```sh
git checkout -b <BRANCH>
```

#### Create a new tracking branch based on a remote branch

```sh
git checkout --track <REMOTE>/<BRANCH>
```

### TAG

#### Tag a commit

```sh
git tag <TAG_NAME>
```

## Commit

### Status

#### List the current changes on a branch

```sh
git status
```

#### Check the differences between the updated state and the initial state of a file

```sh
git diff <FILE>
```

### Updating

#### Stage a file

```sh
git add <FILE>
```

#### Unstage a file 

```sh
git reset HEAD -- <FILE>
```

#### Remove a file

```sh
git rm <FILE>
```

#### Add only some chunks from changes of a file (patch)

```sh
git add -p <FILE>
```

#### Commit all staged changes

```sh
git commit
```

#### Commit all staged changes with an inline commit message

```sh
git commit -m "<COMMIT_MESSAGE>"
```

#### Append all staged changes to the previous commit

```sh
git commit --amend
```

### History

#### Show all commits starting from the latest

```sh
git log
```

#### Show authors, changed date and commit ID for each line in a file

```sh
git blame -c <FILE>
```

### Concatenate commits

#### Squashing multiple commits into one

```sh
git rebase -i HEAD~<NUMBER_OF_COMMITS_FROM_THE_HEAD>
```

### Publishing

#### Retrieve all changes from a remote without applying them

```sh
git fetch <REMOTE>
```

#### Retrieve all changes from a remote and apply them

```sh
git pull <REMOTE> <BRANCH>
```

#### Retrieve all changes from a remote and apply them following a rebase strategy

```sh
git pull --rebase <REMOTE> <BRANCH>
```

#### Commit changes to a remote

```sh
git push <REMOTE> <BRANCH>
```

#### Publish tags to remote

```sh
git push --tags
```

## Integration

### Merge

#### Merge a branch into your current HEAD

```sh
git merge <BRANCH>
```

### Rebase

#### Rebase your current branch

```sh
git rebase <BRANCH>
```

#### Rebase you current branch in interactive mode

```sh
git rebase -i <BRANCH>
```

#### Abort a rebase

```sh
git rebase --abort
```

#### Continue a rebase after resolving conflicts

```sh
git rebase --continue
```

## Undo

### Reset

#### Discard all local changes and start working on the current branch from the last commit
```sh
git reset --hard HEAD
```

#### Reset current branch to a previous commit and discard all changes since then

```sh
git reset --hard <COMMIT_HASH>
```

#### Reset current branch to a previous commit and preserve all changes as unstaged changes

```sh
git reset <COMMIT_HASH>
```

#### Reset current branch to a previous commit and preserve staged local changes

```sh
git reset --keep <COMMIT_HASH>
```

### Checkout

#### Discard local changes to a specific file

```sh
git checkout HEAD <FILE>
```

### Revert

#### Revert a commit by making a new commit which reverses the given commit

```sh
git revert <COMMIT_HASH>
```
