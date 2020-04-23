# Introduction to git

##### is git installed?

```shell
git --version
```


##### syntax:
```shell
git [command] [---flags] [arguments]
```

##### get help
```shell
git help [command]
```

concise help
```shell
git help [-h]
```

##### change a commmand's behaviour
```shell
-f, --flag
|                              # or
[optional]                     # optional
<placeholder>                  # placeholder
[<optional placeholder>]       # optional placeholder
()                             # grouping
--                             # disambiguates the command
...                            # multiple occurences are possible
```

### Getting started with git

##### set username and email
this will be shown when making commits as well
```shell
git config [local|--global|--system] <key> [<value>]
git config --global user.name "martinthoma"
git gonfig --global user.email "martinthoma@email.de"
```

##### read username and emai
```shell
git config user.name
git config user.email
```

### creating a local repository
```shell
mkdir repos                     # repos folder containing all git repositories
cd repos
mkdir projecta                  # one project to work on
cd projecta
git init                        # initialize (create) a repository
projecta ls -a                  # to see that .git file is inside
```

### commit to a local repository
```shell
git status                      # to view status of files in working tree and staging area.
                                # Shows branch, chich commits and whats in the working tree
touch file_name.txt             # adding content to workingtree, normaly done automatically when file
                                # is created/changed in the repo folder
git add <file-or-directory>     # adding untracked or modified file to the staging area. staged content
                                # is part of the next commit, can be seen when git status as added file/dir
git add .                       # adding all files that were touched at ones
                                # when staget files are modified after staging, they have to be added again
                                # to make changes apply.
git status -s                   # ensures that only a short message is delivered by git
git commit -m "commit messsage" # commit content to the local repository. if -m and message not specified,
                                # the local editor will open and one has more spyce to type a commit message
                                # git status now shows a clean working tree and staged area again
git log                         # view the local commit history
git log -oneline                # returning a shorter summary
git log -oneline -2             # 2 is here specifying how many commits it should go back in log
```

### create a remote repository
can easily be done with the online repository provider

### push to a remote repository
One has to ask: Do I have a local repository already?
no  -- clone the remote repository
yes -- add the remote repository

##### (1) no case -- git clone
The clone of a remote repository is a local copy of a remote repository. You can clone any reporitory if you have the URL.

```shell
git clone <url/to/projectname.git> [localprojectname]    # create a local copy of the remote repository.
                                                         # important be in the repos directory when cloning
git remore --verbose                                     # displays information about the remore repositories
                                                         # associated with the local repository
```

##### (2) yes case -- git remote add
add a remote repository to a existing local repository
```shell
git remote add <name> <url>
```
from existing local repo:
```shell
git remote add origin https://githubblabla      # now origin is the name of the repo so that
                                                # we dont have to use the url all the time we push
git remote -v                                   # returns information about the association
```

### pushing to a remote repository
All commits belong to a branch (by default the master branch). Updating the remote repository with the local repository's stuff
```shell
git push                                        # writes commits for a branch to a remote repository
git push [-u] [<repository>] [<branch>]
<repository>                                    # can be a name (shortcut) or url
-u                                              # track this branch (--set-upstream) git will inform you
                                                # when changes happen to the branch
git push -u origin master                       # track the branch (-u) to the origin reporitory to the master branch
```

### The git graph
Getting the git graph showing branches and merges
```shell
git log --oneline --graph
```

##### git IDs
- commit object is a small text file
- annotated tag is a reference to a specific commit
- tree are directories and filenames in the project
- blob is the content of a file in the project
- git ID is the name of a git object (40chars, hashed, unique for given peace of content) is also the name of git objects

```shell
git hash-object <file>      # create SHA-1 object
                            # not used often
-oneline                    # option shortens the git IDs
```

### Git references
Use references instead of SHA-1 hashes
```shell
(HEAD -> Master) # example of 2 references associated with a object
```

##### Branch labels points to the most recent commit in the branch
are implemented as references
There is only one HEAD tag in a repository pointing to the branch of the most recent commit. `454b340 (HEAD -> master) add feature 2` means that the `HEAD` reference points to the `master` reference associated with the latest commit being named `454b340` wich the message `add feature 2`.

```shell
git show HEAD # shows commit info for the current commit
git show HEAD~          # shows commit info for the current commit's parent
                        # same as HEAD~1
git show master~3       # shows the 3rd latest commit of the master branch
git show e89r343~~~     # shows the 3rd parent of the commit with name e89r343

git show HEAD^          # shows the head commits first parent
git show HEAD^^         # shows the head commits first parent's first parent
git show HEAD^2         # shows the head commits second parent
                        # often leads to an error if there are no 2
                        # parents of a single commit (as the case after a merge)
```

##### Tags
  - leightweight
    - simple references
  - annotated
    - full git object that references a commit
```shell
git tag                       # shows tags of commits (versions)
git show v0.1                 # shows commit with tag v0.1
git tag <tagname> [<commit>]  # <commit> defaults HEAD
```


### Branches
By default one commits to the master branch. They are fast and easy to create. Isolates the work of team members allows testing. They enable team work. Supports multiple versions.

```shell
git branch                  # see list of branches
```

##### creating a branch
```shell
git branch <name>           # e.g. git branch featureX
```

##### checkout
Updates the HEAD reference to the branch label. Updates the working tree with the commit's files.
```shell
git checkout <beanch_or_commit>   # to checkout to a branch or commit
# switch to a branch from master branch
# (featureX branch has to be created before)
git checkout featureX
git branch                        # shows that we are currently on
                                  # the featureX branch
ls                                # shows files for featureX

#shortcut
git checkout -b featureX          # creates branch and checkout branch
                                  # at the same time
```

### detached HEADs
### deleting a branch label
is used after a topic branch has been merged successfully

```shell
git branch -d featureX
```


##### undoing a accidential branch delete
```shell
git branch -D featureX                  # deleted branch featureX and
                                        # leaved dangling commit
git reflog                              # adds featureX again but has no tag yet
git checkout -b featureX <commit_name>  # assigns featureX tag to the
                                        # resroted commit
```

### Merging
Combines the work of independent branches.
1. fast forward merge
2. merge commit
3. squash merge
4. rebase


(1)
moves the base branch label to the tip of the topic branch. In the end, the branch is just incluses in the master branch.
steps when performing a fast forward merge
```shell
git log --oneline --graph --all   # showing branches
git checkout master               # switching to master branch
git merge featureX                # by default using the fast forward merge
git branch -d featureX            # deleting the old branch commit cag
```

(2)
if there are commits to the master branch before merging with the feature branch then we can not do a fast forward merge.
1. combines the commits at the tips of the merged Branches
2. places the result in the merge commit
merge commit is the union of the two commits merged
- results in a nonlinear commit graph

```shell
git checkout master
git merge featureX # accept of modify merge message
git branch -d featureX
```

can also create a no fast forward merge to better keep ttrack of branched and merges
```shell
git checkout master
git merge --no-ff featureX # accept of modify merge message
git branch -d featureX
```

### resolving merge conflicts
always when a person has to make a desicion which branch's code to take in one file.

involves three commits
1. the tip of the current branch (B) "ours" or "mine"
2. the tip of the branch to be merged (C) "theirs"
3. 3. a common ancestor (A) "merge base"

1. Checkout master
2. Merge featureX
  1. CONFLICT - both modified same file
1. fix file
2. stage file
3. commit the merge commit
4. delete the featureX beanch label

```shell
git log --oneline --graph --all
git merge featureX                # CONFLICT
git status                        # "your have unmerged fils "
# use any editor to go in the conflicted file and resolve the conflicts
cat file.txt
git add file.txt
git commit
git branch -d featureX
```

### fetch, pull, push
network commands communicate with the remote repository

- clone - copies a remote directory
- fetch - retrieves new objects and references from the remote repository
-  pull - fetches and merges commity locally
- push - adds new objects and fererences to the remote repository

##### fetch
after fetching the git status command will tell is we are behind the master brand

not that important
##### pull
if objects are fetched, the tracking beanch is merged to the current local branch.
can also specify merging options
``` shell
git pull

--ff
--no-ff
--ff-only
--rebase
```
pull basically keeps track of everything and merges all together so that after pull one has the actuall project status from the remote repository

example
```shell
git status # shows we are behind
git pull # takes care
git los --oneline # shows that pull has taken care
```

##### push 
```shell
git push -u origin master # pushing to remote repository
```

rule: always pulling before pushing
