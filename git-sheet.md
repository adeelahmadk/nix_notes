# GIT Command Reference

This document is a reference for a beginner. It's by no means an exhaustive listing of git commands. For an in-depth learning and reference use the [Pro Git book by Scott Chacon and Ben Straub, Apress 2014](https://git-scm.com/book/en/v2). 

## Contents

- [Configuration](#Configuration)

- [States in which file might exist](#States-in-which-file-might-exist)

- [Start a new Project](#Start-a-new-Project)

- [Making changes](#Making-changes)

- [Setting up SSH access](#Setting-up-SSH-access)

- [Configure multiple Versioning Hosts](#Configure-multiple-Versioning-Hosts)

- [Push to upstream repo cloned prior to ssh key setup](#Push-to-upstream-repo-cloned-prior-to-ssh-key-setup)

- [Regular work-flow maintenance](#Regular-work-flow-maintenance)

    - [Connect/View/Change remote repository](#Connect-View-Change-remote-repository)

    - [Remove a file from Staging or Commit](#Remove-a-file-from-Staging-or-Commit)

    - [Push/Pull Repos](#Push-Pull-Repos)

    - [Viewing project history](#Viewing-project-history)

    - [Branches](#Branches)

        - [Renaming the default branch (master)](#Renaming-the-default-branch)

    - [Tags](#Tags)

    - [Sync with upstream changes](#Sync-with-upstream-changes)

    - [Git Clone](#Git-Clone)

        

## Configuration

Setup git with your name and email address:
```sh
git config --global user.name "Your Name"
git config --global user.email "your@email.com"
```
When `push.default` is set to '*matching*', git will push local branches to the remote branches that already exist with the same name.
```sh
git config --global push.default matching
```
When `push.default` is set to '*simple*', git only pushes the current branch to the corresponding remote branch that`git pull`uses to update the current branch.

```sh
git config --global push.default simple
```
View git configurations:
```sh
git config --list
```

## States in which file might exist

| State | Description |
| ----- | ----------- |
|Untracked|When you first create the file, it goes in this area|
|Staged/index|When you use git add command on the file, it goes in this area|
|Committed|When you use the git commit on the file, it goes in this area|
|Modified|The file is committed but has the local changes which are not committed or staged yet|

## Start a new Project

Initialize a new project repository in git:

```sh
mkdir testrepo
cd testrepo
git init
```

### Making changes

Take a snapshot of the contents of files under the current directory and store to index:
```sh
  git add .
  git add file1 file2 file3
  git add <file | dir>
```

See what is about to be committed:
```sh
git diff --cached
```
Without `--cached`, `git diff` will show you any changes that you’ve made but not yet added to the index:
```sh
git diff
```
Permanently store the contents of the index in the repository:
```sh
git commit
```
Automatically notice any modified (but not new) files, add them to the index, and commit, all in one step:
```sh
git commit -am "commit message"
```
Add a remote repo before pushing to it:
```sh
git remote add origin https://github.com/username/testrepo.git
```
View your `remote`'s `fetch`/`push` urls

```sh
git remote -v
```

Push repo to the remote host:

```sh
git push origin master
```

## Setting up SSH access

List the files in your .ssh directory, if they exist

```sh
    ls -al ~/.ssh
```

Creates a new ssh key, using the provided email as a label
```sh
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```

To generating public/private RSA key pair, start the ssh-agent in the background

```sh
eval "$(ssh-agent -s)" 
Agent pid 59566
```

Add your SSH key to the ssh-agent
```sh
ssh-add ~/.ssh/id_rsa
```

Attempts to ssh to GitHub
```sh
ssh -T git@github.com
```

Before switching remote URLs, list

```sh
git remote -v
```

Change to HTTPS
```sh
git remote set-url origin https://github.com/USERNAME/OTHERREPOSITORY.git
```

Change to SSH
```sh
git remote set-url origin git@github.com:USERNAME/OTHERREPOSITORY.git
```



## Configure multiple Versioning Hosts

Create SSH Keys
```sh
ssh-keygen -t rsa -C "github email"
```
Enter *passphrase* when prompted. If you see an option to save the *passphrase* in your *keychain*, do it for an easier life. Save keys to: `~/.ssh/id_rsa_gh`. Repeat for bitbucket:

```sh
ssh-keygen -t rsa -C "bitbucket email"
```
Save bitbucket key to `~/.ssh/id_rsa_bb`

Attach Keys
```sh
xclip -sel clip < ~/.ssh/id_rsa_gh.pub
xclip -sel clip < ~/.ssh/id_rsa_bb.pub
```
Paste into text area, under ssh settings, in your github or bitbucket account. Now, create `config` file (enter your editor here if different)

```sh
vim ~/.ssh/config
```
 Create your git aliases like so:

```
 #Github (default)
 Host gh
 HostName github.com
 User git
 IdentityFile ~/.ssh/id_rsa

#Bitbucket (secondary)
 Host bb
 HostName bitbucket.org
 User git
 IdentityFile ~/.ssh/id_rsa_bb

 # Must add this line if you want to be able to ssh 
 # in to your local machines
 #Others
 Host *
 IdentityFile ~/.ssh/id_rsa
 IdentitiesOnly yes
```

Add the identities to SSH:
```sh
ssh-add ~/.ssh/id_rsa_gh
ssh-add ~/.ssh/id_rsa_bb
```

Enter *passphrase* if prompted. Check keys were added:
```sh
ssh-add -l
```

Check that repo recognizes keys:
```sh
ssh -T gh
ssh -T bb
```

### Push to upstream repo cloned prior to ssh key setup
```sh
git push git@gh:username/repo
```
or
```sh
git push git@bb:sername/repo
```

## Regular work-flow maintenance

### Connect View Change remote repository

Connect to repository `remote_repo`(on GitHub etc.) in branch `master` with **user_name**

```sh
git remote add origin https://github.com/user_name/remote_repo.git
```

View your `remote`'s `fetch`/`push` URL

```sh
git remote -v
```

Change your `remote`'s URL to HTTPS

```sh
git remote set-url origin https://github.com/USERNAME/OTHERREPOSITORY.git
```

Change your `remote`'s URL to SSH

```sh
git remote set-url origin git@github.com:USERNAME/OTHERREPOSITORY.git
```

### Remove a file from Staging or Commit

To remove from staging, we can use following command:
```sh
git rm --cached <file_name>
```
Here, we are using the rm command along with switch --cached which indicates the file to be removed from the staging or cached area. For example, we can use following command-
```sh
git rm --cached unwanted_file.txt
```
Removing file from committed area requires 3 commands to be run, they are as follows:
```sh
git reset --soft HEAD^1
```
Above will undo the latest commit and bring your files in the staging area. Now, we can easily remove it from staging area, as mentioned from previous point.
```sh
git rm --cached <file-name>
```
By running above command, the file will appear in the untracked file section.

### Push Pull Repos

Push the *committed* contents of local repository into the remote host repository (GitHub etc.) in branch origin/master

```sh
git push origin master
```

Pull the `remote` host repository (GitHub etc.) contents into the local repository from branch origin/master.

```sh
git pull origin master
```

### Viewing project history

At any point view the history of your changes:
```sh
git log
```
See complete `diff` at each step:
```sh
git log -p
```
Overview of the change is useful to get a feel of each step:
```sh
git log --stat --summary
```

### Branches

A single Git repository can maintain multiple branches of development.

List branches or create a new branch (named "branch-name") in a repository

```sh
git branch [branch-name]
```

List of all existing local branches:
```sh
git branch
```
List of all existing remote branches:
```sh
git branch -r
```
To list of all existing local+remote branches:
```sh
git branch -a
```
To switch to the experimental branch:
```sh
git checkout experimental
```
To merge the changes made in experimental into master:
```sh
git merge experimental
```

If there are conflicts, markers will be left in the problematic files showing 
the conflict;

```sh
git diff
```
Once you’ve edited the files to resolve the conflicts,
```sh
git commit -a
git push origin master
```
At this point you could delete the experimental branch with (ensures that the 
changes in the experimental branch are already in the current branch.):
```sh
git branch -d experimental
```
If you develop on a branch crazy-idea, then regret it, delete the branch with:
```sh
git branch -D crazy-idea
```

#### Renaming the default branch

##### For personal repository

To rename the default branch for your personal repository

```sh
# rename your local branch
git branch -m master main
# push renamed branch upstream & set remote tracking branch
git push -u origin main
```

Then log into the upstream repository host (GitHub, GitLab, Bitbucket, etc.) and change the **default branch**.

```sh
# delete the old branch upstream
git push origin --delete master
# update the upstream remote's HEAD
git remote set-head origin -a
```

##### For a cloned repository

If the branch was renamed

```sh
# fetch the latest branch
git fetch --all
# update the upstream remote's HEAD
git remote set-head origin -a
# switch your local branch to track the new remote branch
git branch --set-upstream-to origin/main
# rename your branch locally
git branch -m master main
```

### Tags

When working with Git, it is quite common for developers to create tags in order to have reference points in your development. Tags are created in order to have references to release versions for example.

#### Checkout Git Tag

In order to checkout a Git tag, use the “git checkout” command and specify the `tagname` as well as the `branch` to be checked out:

```sh
git checkout tags/<tag> -b <branch>
```

To fetch tags from your remote repository, use “git fetch” with the “–all” and the “–tags” options.

```sh
$ git fetch --all --tags

Fetching origin
From git-repository
   98a14be..7a9ad7f  master     -> origin/master
 * [new tag]         v1.0       -> v1.0
```

For example that you have a tag named “v1.0” that you want to check out in a branch named “release”

```sh
$ git checkout tags/v1.0 -b v1.0-branch

Switched to a new branch 'v1.0-branch'
```

You can inspect the state of your branch by using the “git log”  command. Make sure that the HEAD pointer (the latest commit) is pointing to your annotated tag.

```sh
$ git log --oneline --graph

* 53a7dcf (HEAD -> v1.0-branch, tag: v1.0) Version 1.0 commit
* 0a9e448 added files
* bd6903f (release) first commit
```

#### Checkout latest Git tag

First update your repository by fetching the remote tags available.

```sh
$ git fetch --tags

Fetching origin
From git-repository
   98a14be..7a9ad7f  master     -> origin/master
 * [new tag]         v2.0       -> v2.0
 * [new tag]         v1.0       -> v1.0
```

Then, retrieve the latest tag available by using the “git describe” command.

```sh
$ tag=$(git describe --tags `git rev-list --tags --max-count=1`)

$ echo $tag
v2.0
```

Finally, use the “git checkout” command to checkout the latest git tag of your repository.

```sh
$ git checkout $tag -b latest

Switched to a new branch 'latest'
```

You can execute the `git log` command in order to make sure that you are actually developing starting from the new tag.

```sh
$ git log --oneline --graph

* 7a9ad7f (HEAD -> latest, tag: v2.0, origin/master, master) version 2 commit
* 98a14be Version 2 commit
* 53a7dcf (tag: v1.0, v1.0-branch) Version 1.0 commit
* 0a9e448 added files
* bd6903f (branch3) first commit
```

### Sync with upstream changes

```sh
git fetch origin master
git diff master origin/master
git merge origin/master
git push origin master
```



## Git Clone

### How to clone a specific tag with Git

The idea here is to clone a repository using `git-clone` command and then checkout the specific tag using `git-checkout`.

```sh
# clone the remote repository
git clone <repository> .
 
# switch to the specific tag
git checkout tags/<tagname>
```

Note, this will leave the repository in a ‘detached HEAD’ state. This means any commits you make in this state will not impact any branches. If needed, you can create a new branch using `-b` option:

```sh
git checkout tags/<tagname> -b <branchname>
```

You can also checkout the specific tag with `git-clone`. It has –branch option which can also take tags and detaches the HEAD at that commit in the resulting repository.

```sh
git clone -b <tagname> <repository> .
```

If you only need the specific tag, you can pass `--single-branch` flag which prevent fetching all the branches in the cloned repository. With the `--single-branch` flag, only the branch/tag specified by the `--branch` option is cloned.

```sh
git clone -b <tagname> --single-branch <repository> .
```

Alternatively, you can specify the `--depth` option which limits the number of commits to be downloaded to the specified depth. It implies `--single-branch` by default.

```sh
git clone -b <tagname> --depth 1 <repository> .
```

