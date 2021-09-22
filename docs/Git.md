# Code revisioning with `git`

### Table of content


<!-- vim-markdown-toc GFM -->

* [Branches](#branches)
	* [Cloning](#cloning)
	* [Merge a fix](#merge-a-fix)
	* [List branches](#list-branches)
	* [Remote Tracking](#remote-tracking)
	* [Rename a branch](#rename-a-branch)
	* [Further](#further)
* [Origin and remote updates](#origin-and-remote-updates)
	* [Update and synchronisation](#update-and-synchronisation)
	* [Remote management](#remote-management)
* [Push](#push)
* [Pull](#pull)
* [Diffs](#diffs)
* [Merge conflicts](#merge-conflicts)
* [Merge and rebase](#merge-and-rebase)
* [Debugging](#debugging)
	* [log](#log)
	* [blame](#blame)
	* [bisect](#bisect)
* [Subtree](#subtree)
* [Submodule](#submodule)
	* [add a submodule](#add-a-submodule)
	* [clone a repo with a submodule](#clone-a-repo-with-a-submodule)
	* [update your SM repo](#update-your-sm-repo)
	* [publish your submodule changes](#publish-your-submodule-changes)
	* [Example:](#example)
	* [Recursive status/commit](#recursive-statuscommit)
* [Stash](#stash)
* [Tag](#tag)
* [Reset](#reset)
* [Cherry-pick](#cherry-pick)
* [Patch](#patch)
* [Hooks](#hooks)
* [Notes](#notes)
* [Changes requests](#changes-requests)
	* [Request-pull](#request-pull)
	* [Merge requests](#merge-requests)
* [Philosophy and issues](#philosophy-and-issues)
	* [Workflows](#workflows)
	* [Fast-forward or not](#fast-forward-or-not)
	* [Commit early commit often](#commit-early-commit-often)
* [Change history](#change-history)
	* [Edit last commit](#edit-last-commit)
	* [Edit earlier commit](#edit-earlier-commit)
	* [Discard modifications](#discard-modifications)
	* [Merge correction](#merge-correction)
	* [Author name](#author-name)
* [Removing a file from history](#removing-a-file-from-history)
* [Split a repository](#split-a-repository)
* [Merging two repositories](#merging-two-repositories)
* [Submodule merge into main repo](#submodule-merge-into-main-repo)
* [Recover data and garbage collector](#recover-data-and-garbage-collector)
* [Tree visualisation](#tree-visualisation)
* [Configuration](#configuration)
* [Migrate a git repo](#migrate-a-git-repo)
* [Install latest git](#install-latest-git)
* [Orphaned commit](#orphaned-commit)
* [Sync branches local/remote](#sync-branches-localremote)
* [Prevent from pushing a local branch](#prevent-from-pushing-a-local-branch)
* [Change origin url](#change-origin-url)
* [Signed commits](#signed-commits)
* [End-of-line](#end-of-line)
* [Merge taking always the remote](#merge-taking-always-the-remote)
* [Merge single file / partial merge](#merge-single-file--partial-merge)
* [Git platforms](#git-platforms)
	* [Gitolite: git on server](#gitolite-git-on-server)
		* [Wild repo](#wild-repo)
		* [List of available repos](#list-of-available-repos)
		* [Rename repo](#rename-repo)
		* [Rights](#rights)
		* [Remote commands](#remote-commands)
		* [Multiple keys](#multiple-keys)
		* [Aliases/groups](#aliasesgroups)
	* [Github](#github)
	* [GitLab](#gitlab)
		* [Merge requests](#merge-requests-1)
* [Tig](#tig)
* [Statistics](#statistics)

<!-- vim-markdown-toc -->

### Branches

#### Cloning

Cloning and getting to another branch

```bash
git clone me@server:repo
git checkout -b branch-name
```

now working on another branch with the name branch-name can do git commit, push, etc.

#### Merge a fix

Receive info that a fix has come.

1. Switching back to master:

  ```bash
  git checkout master
  ```

2. Creating a new branch

  ```bash
   git checkout -b fix-branch v0.1
   #change everything one needs
   git commit -a -m "fixed!"
   ```

3. Merge new branch into master

  ```bash
  git checkout master         # back to master branch
  git merge fix-branch        # fast forward the master branch
  ```

4. Delete temporary branch and back to working branch

  ```bash
  git checkout -d fix-branch  # delete the fix-branch
  git checkout branch-name    # back to working branch
  ```


the fix is not in the branch-name. This could be done by

```bash
git merge master
```

or the branch-name later in master as

```bash
git checkout master
git merge branch-name
```

#### List branches

```bash
git branch      # lists the branches
git branch -a   # lists the branches (incl. remote)
git branch -v   # shows the last commit on each branch
```

`--merged` and `--no-merged` show which branches have been merged on the current

#### Remote Tracking

We can check whether some local branch are set to track remote branch (as [How can I see which Git branches are tracking which remote / upstream branch?](https://stackoverflow.com/a/4952368/3337196))

```bash
git branch -vv
```

Set the tracking (see [Adding a tracking branch](https://githowto.com/adding_a_tracking_branch))

```bash
git branch --track local_branch origin/remote_branch
```

or if the local branch already exists

```bash
git branch --set-upstream-to=origin/remote_branch local_branch
```

Remove tracking (see [](https://stackoverflow.com/a/3046478/3337196))

```bash
git branch --unset-upstream
```

or

```bash
git branch -d -r origin/remote_branch
```

Update all tracked branches

```bash
git pull --all
```

It is also possible to check the status of each branches ([Show git ahead and behind info for all branches, including remotes](https://stackoverflow.com/q/7773939/3337196)), for example, using more recent git versions

```bash
git for-each-ref --format="%(refname:short) %(upstream:track)" refs/heads
```

Update a branch without checking it out can be done (when doing a fast-forward)

```bash
git fetch origin master:master
```

See also [Merge, update, and pull Git branches without using checkouts](https://stackoverflow.com/a/17722977/3337196).

#### Rename a branch

a local branch:

```bash
git branch -m oldname newname
```

or

    git branch -m newname # rename current branch

See [Rename local git branch](https://stackoverflow.com/questions/6591213/rename-local-git-branch#6591218).

a remote branch:

 ```bash
 git branch -m br1 br2
 git push origin :br1 # deletes remote branch
 git push origin br2
 ```

and another `br1` may be created. But careful with possible other versions of `br1` checked out

See
- [Rename a remote branch on github](http://blog.changecong.com/2012/10/rename-a-remote-branch-on-github/)
- [Rename master branch for both local and remote git repositories](https://stackoverflow.com/questions/1526794/rename-master-branch-for-both-local-and-remote-git-repositories/1527004#1527004)

#### Further

See also [Basic Branching and Merging](http://www.git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging) and following pages.

Note: If a branch is deleted, the "special" keyword which is a reference to that branch disappears. But the corresponding hash/commit still exist. Creating a branch with the same name is possible but will not have any link to the previous one. However running

```bash
git checkout -b branch <hash>
```

allows to re-create the branch. A branch name should be seen as a C-pointer. It gives a direct access to a given <hash>, which has not "memory" of where it was pointing to before.

### Origin and remote updates

#### Update and synchronisation

Clone

```bash
git clone me@gitserver:project.git
```

(do some work and some commits)
someone update the server's version

```bash
git fetch origin
```

this makes two different branches locally: master and origin/master which could be merged
if there is a branch named foo on the server, you will have a read only origin/foo branch locally then either

```bash
git merge origin/foo
```

or

```bash
git checkout -b foo origin/foo # creates a local version of the foo branch which can be worked on
```

See also [tracking](#remote-tracking).

#### Remote management

There are a few commands to manage the remote(s). Like

```bash
git remote show		# List the remotes names (typically origin, and other(s) if available)
git remote show origin	# Provides details about the remote (needs connection)
git remote add remote-name https://github.com/bilbopingouin/docs-git.git  # Add a new remote (no connection required)
git remote rm remote-name # Remove the link to a specific remote
git remote rename old-name new-name # Change the reference name to the remote
git remote get-url remote-name	# Display the URL
git remote set-url remote-name https://github.com/bilbopingouin/docs-git.git  #	Update the link
```

And generally, running

```bash
git remote --help
```

provides some more information.

### Push

Push a local branch to the remote

```bash
git push origin branch-name
git push origin --delete branch-name # does the same as git checkout -d branch-name but on the remote rep
```

Updating a remote branch

``` bash
git push --force origin branch-name
git push --force-with-lease branch-name # Only overwrite if the local branch is not outdated
```

### Pull

`git pull` runs `git fetch` and `git merge origin/foo` ([Git fetch and merge](http://longair.net/blog/2009/04/16/git-fetch-and-merge/)).

due to my global setting of `--no-ff` and `--no-commit` for master branch, when I run 

    git checkout master
    git pull origin master

it stops before commiting. One way to clarify that would be 

```bash
git checkout tmp
git commit -am "results of pull"
git checkout master
git merge --ff --ff-only origin/master
```

and you get back to the full remote tree with an extra commit, that you can forget about... which incidently has the same status as the origin/master

alternatively one can run

    git pull --ff --commit

### Diffs


Only show the list of files that differ

    git diff --name-only branch

Use no pager

    git --no-pager diff --name-only branch

Ignore the whitespaces

    git diff -w <commit>

See also [Ignore all whitespace changes with git diff between commits](https://stackoverflow.com/questions/33159394/ignore-all-whitespace-changes-with-git-diff-between-commits)

### Merge conflicts

when a git merge ended in CONFLICT

```bash
git status # shows where the conflict lies
```

fix the errors

```bash
git add file-which-conflicted.c
```

alternatively one can use

    git mergetool

And then, in both cases

    git commit

### Merge and rebase


if we have the following

    C0 <--- c1 <--- c3  (b1)
                 `- c2  (b2)
One can use

    git checkout b1
    git merge b2

Which then produces

    C0 <--- c1 <--- c3 <--- c4  (b1)
                 `- c2 <-'      (b2)

An alternative is

    git checkout b2
    git rebase b1

Which leads to

    C0 <--- c1 <--- c3 <-        (b1)
                 `- c2   `- c3'  (b2)

where the steps from c1 to c2 are reapplied on c3.

At this step a merge on b1 would make b1 and b2 point to the same state.

One big difference is that in case of rebase, the maintener of b2 has to deal with the integration, whereas in merge, it is the maintainer of b1. It makes a cleaner history.

But rebasing should only be done when the parallel branch is local (only!), see [Branching Rebasing](http://www.git-scm.com/book/en/v2/Git-Branching-Rebasing)

On the merge vs rebase policy, see
- [Are you a merge or rebase guy](http://blog.marcomonteiro.net/post/are-you-a-merge-or-rebase-guy)
- [Team workflows: merge or rebase](http://blogs.atlassian.com/2013/10/git-team-workflows-merge-or-rebase/)
- [When use git rebase instead of git merge](https://stackoverflow.com/questions/804115/when-do-you-use-git-rebase-instead-of-git-merge)
- [rebase vs merge questions](https://stackoverflow.com/questions/457927/git-workflow-and-rebase-vs-merge-questions)

Good options for seeing conflicts:

    git config --global merge.conflictstyle diff3

`git rebase` with more options

```bash
git rebase --interactive master # runs a rebase with some more options
```

see also [Squash commits](http://davidwalsh.name/squash-commits-git):

    git merge --squash branch

will merge the branch into the current branch grouping its different commits into one ([Git merge](http://www.git-scm.com/docs/git-merge))

### BASE / LOCAL / REMOTE

Considering the following schematics,

```
c0 <--- c1 <-- c3 
            `- c2 (HEAD)
```

To combine c2 and c3, we can either merge or rebase (see also the respective sections above). I want to point out to the process names.
- merge:
  ``` bash
  git merge c3
  ```
  BASE is c1, LOCAL is c2 and REMOTE is c3.  
  *ours* is c2 and *theirs* is c3.
- rebase: 
  ``` bash
  git rebase c3
  ```
  BASE is c1, LOCAL is c3 and REMOTE is c2.  
  *ours* is c3 and *theirs* is c2.

### Debugging

Ref. [Debugging with git](http://git-scm.com/book/en/v2/Git-Tools-Debugging-with-Git)

#### log

`git log` allows to search the history for various things. For example

- Find when a file was deleted ([on SO](https://stackoverflow.com/a/16635324/3337196))

  ```bash
  git log --full-history -- path/to/file
  ```

#### blame

    git blame -L first,last filename 

checks who and when the lines from first to last of filename were edited

#### bisect

`git bisect` help to go through versions to find out when a bug was introduced.
using dichotomy and good/bad markers git helps to find the revision where a but was introduced. And then git prints info about the given revision.

```bash
git bisect start                    # starting bisect
git bisect bad                      # indicate that the current version is buggy
git bisect good <hash|version>      # indicate that a previous version is correct. It will jump directly to another version.
```

repeat at each steps...

```bash
git bisect good                     # test/check and define good/bad. Each time, it checks out another version
git bisect bad
```
        
ends with

```bash
git bisect reset                    # end the bisect and reset back HEAD to its original version
```

Alternatively, one can use a script that checks directly. The script should return 0 if good and non-0 otherwise, one can do:

```bash
git bisect start <bad hash> <good bash>
git bisect run test.sh
```



### Subtree

split a large git repo into individual smaller repos, see [Detach subdirectory](https://stackoverflow.com/questions/359424/detach-subdirectory-into-separate-git-repository)

### Submodule


#### add a submodule

```bash
git clone project
cd project
git submodule add sm
git commit -am "added submodule sm"
```

one can also

```bash
git submodule add https://github.com/jmanoel7/vim-games.git plugins/vim-games
```

#### clone a repo with a submodule

```bash
git clone project
cd project
git submodule init
git submodule update
```

or

```bash
git clone --recursive project
```

#### update your SM repo

```bash
cd project/sm
git fetch
git merge origin/master
cd ..
git commit -am "sm actualised"
```

or

```bash
git submodule update --remote sm
```

#### publish your submodule changes

```bash
git push --recurse-submodule=check
```

it will do a push of project, but only after making sure that no local modification to sm need to be published

See [Submodules](http://www.git-scm.com/book/en/v2/Git-Tools-Submodules)

#### Example:

- Create `repo1`

  ```bash
  mkdir repo1
  cd repo1
  git init --bare
  cd ..
  ```

- Add content

  ```bash
  git clone repo1 repo1_loc
  touch a
  git add a
  git commit -m "adding a"
  git push origin master
  cd ..
  ```

- Create `repo2`

  ```bash
  mkdir repo2
  cd repo2
  git init --bare
  cd ..
  ```

- Add content

  ```bash
  git clone repo2 repo2_loc
  cd repo2_loc
  touch b
  git add b
  git commit -m "adding b"
  git submodule add $PWD/../repo1 repo1
  git commit -m "submodule added"
  git push origin master
  cd ..
  ```

- Get another local copy of `repo2`

  ```bash
  git clone repo2 loc
  cd loc
  git submodule init 
  git submodule update
  cd ..
  ```

- Update `repo2` on loc

  ```bash
  cd repo2_loc
  touch c
  git add c
  git commit -m "adding c"
  git push origin master
  cd ../loc
  git fetch origin
  git merge --ff-only origin/master
  cd ..
  ```

- Update `repo1` on loc

  ```bash
  cd repo1_loc
  touch d
  git add d
  git commit -m "adding d"
  git push origin master
  cd ../loc
  cd repo1
  git fetch --all
  git merge --ff-only origin/master
  cd ..
  git add repo1
  git commit -m "submodule update"
  git push origin master
  cd ..
  ```

- Modify `repo2` on loc and sync on `repo2_loc`

  ```bash
  cd loc
  touch e
  git add e
  git commit -m "adding e"
  git push origin master
  cd ..
  cd repo2_loc
  git pull --commit --ff-only origin master
  git submodule update
  cd ..
  ```

- Modify `repo1` on loc and sync on `repo1_loc`, `repo2_loc`

  ```bash
  cd loc
  cd repo1
  touch f
  git add f
  git commit -m "adding f"
  git push origin master
  cd ..
  git add repo1
  git commit -m "f added to sub"
  git push origin master
  cd ..
  cd repo1_loc
  git fetch --all 
  git merge --ff-only origin/master
  cd ..
  cd repo2_loc
  git fetch --all
  git merge --ff-only origin/master
  git submodule update
  cd repo1
  git checkout master
  git merge --ff-only origin/master
  cd ..
  cd ..
  ```

#### Recursive status/commit

The status of all submodules can be found as

```bash
git submodule foreach git status
```

And a commit can be done as

```bash
git submodule foreach 'if [ -n "$(git status --porcelain)" ]; then git commit -am "Some message"; fi'
```

will run commit on all submodules, only if they are not clean.

### Stash


Save modifications as temporary "commit"

    git stash

List temps

    git stash list

Edit the stash

    git stash pop [<stash>]
    git stash apply [<stash>]

Discard the last stash 

    git stash drop [<stash>]

Make a branch out of a stash

    git stash branch branchname

Inspect the stashes

```bash
git stash show -p               # diff to last stash
git stash show -p stash@{0}     # same
git diff stash@{0} master       # compare with a given branch
```

It is also possible control what gets added to the stash, for example
	    
```bash
git stash -u   # or --include-untracked includes the untracked files into the stash
git stash -a   # or --all also includes the files that would be ignored with the .gitignore
git stash -p   # runs interactively through the modifications to see if we want to include them in the stash or not	    
```
	   

See also:
- [Git diff against a stash](https://stackoverflow.com/q/7677736/3337196)
- [Git stash](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)

### Tag

Tagging a commit

```bash
git tag v1.4-lw # lightweight tag
git tag -a v1.4 -m "version 1.4" # annotated tag
git tag -a v1.2 55646830 # tag the previous commit with hash 55646830
```

Sharing a tag

```bash
git push origin v1.5 # share the tag on the remote, alternatively
git push origin --tags # share all the tags
git checkout -b version2 v2.0.0 # creates the branch version2 which corresponds to the v2.0.0
```

See [Basic tagging](http://git-scm.com/book/en/v2/Git-Basics-Tagging)

Also in combination with describe:

```bash
echo hello > foo
git add foo
git commit -m "foo"
git tag -a v0.1 -m "first draft"
echo world >> foo
git commit -am "update 1"
echo "or not" >> foo
git commit -am "update 2"
git describe master # gives 'v0.1-2-g12fc6be' which is 2 commits since v0.1 and hash 'g12fc6be'
```

List tags

```bash
git for-each-ref --sort="-taggerdate" --format="* %(color:yellow)%(object) %(color:white) %(taggerdate) [%(creator)] %(color:green) %(refname:short) %(color:red) %(subject)" refs/tags
```

Remove tag

```bash
git tag -d tagname
git tag --delete tagname
```

Remove remote tag

```bash
git push --delete origin tagname
```

### Reset


can unstage changes

```bash
git reset HEAD filename # reverts the git add filename can undo the last commit and unstage the modifications
git reset HEAD~
```

Read
- [Git reset](http://www.git-scm.com/docs/git-reset)
- [Undoing things](http://www.git-scm.com/book/en/v2/Git-Basics-Undoing-Things)
- [Reset demystified](http://www.git-scm.com/book/en/v2/Git-Tools-Reset-Demystified)

We have then

```bash
git commit -m "v2"
echo world >> a
git add a
git commit -m "v3"

git reset --soft  HEAD~ # undo the last commit, but leaves git add (staged)
git reset --mixed HEAD~ # undo commit and add but leaves world in a: [default]
git reset --hard  HEAD~ # reverts to the status after the commit v2
```

E.g. 

- Creating a directory with last modification

  ```bash
  git init
  touch a
  git add a
  git commit -m "Creating file a"
  echo foo >> a
  git add a
  git commit -m "foo in a"
  echo bar >> a
  git add a
  git commit -m "bar in a"
  ```
        
- Situation

  ```bash
  git status
     On branch master
     nothing to commit, working tree clean

  git --no-pager tree
     * 65fc7f4 2018-09-11 (HEAD -> master) [bilbopingouin, N] - bar in a
     * e0e202f 2018-09-11 [bilbopingouin, N] - foo in a
     * 697194d 2018-09-11 [bilbopingouin, N] - Creating file a

  cat a
     foo
     bar
  ```
            
- Effect of `git reset --soft HEAD~`

  ```bash
  git status
	On branch master
	Changes to be committed:
	    (use "git reset HEAD <file>..." to unstage)

	      modified:   a
  git --no-pager tree
      * e0e202f 2018-09-11 (HEAD -> master) [bilbopingouin, N] - foo in a
      * 697194d 2018-09-11 [bilbopingouin, N] - Creating file a

  cat a
	foo
	bar
  ```
            
 - Effect of `git reset --mixed HEAD~`

      git status
            On branch master
            Changes not staged for commit:
                (use "git add <file>..." to update what will be committed)
                (use "git checkout -- <file>..." to discard changes in working directory))

                  modified:   a
      git --no-pager tree
          * e0e202f 2018-09-11 (HEAD -> master) [bilbopingouin, N] - foo in a
          * 697194d 2018-09-11 [bilbopingouin, N] - Creating file a

      cat a
            foo
            bar
            
- Effect of `git reset --hard HEAD~`

  ```bash
  git status
	On branch master
	nothing to commit, working tree clean

  git --no-pager tree
      * e0e202f 2018-09-11 (HEAD -> master) [bilbopingouin, N] - foo in a
      * 697194d 2018-09-11 [bilbopingouin, N] - Creating file a

  cat a
	foo
  ```
            
Instead of calling with `HEAD~`, calling reset HEAD does:
- `--soft`: nothing
- `--mixed`: reverse git add
- `--hard`: same as git checkout HEAD (discard all modifs)

afterward, a

```bash
git reset --soft b84dd89
```

can be used to go back to where we were before the reset. The hash can be found in `git reflog`.

discard changes since last commit on a file

```bash
git checkout -- path/to/file.c
```

revert last commit local and remote

```bash
git reset HEAD^   # remove last commit locally, but keep the changes. Use --hard to discard them completely
git push --force  # One should be careful that no one has synchronised their copy! 
```


### Cherry-pick


reproduces a given commit (like rebase for a single commit)

Setting some environment

```bash
mkdir dir
cd dir
git init
echo hello > foo
git add foo
git commit -m "foo"
git checkout -b branch
echo world >> foo
git commit -am "foo updated"
echo bla > bar
git add bar
git commit -m "bar"
```

Cherry-picking

```bash
git log pretty=oneline # check the commit that one wants to reproduce
git checkout master
git cherry-pick 01234....
```

Result

```bash
$ cat foo
hello
world
```

It is also possible to cherry-pick a range of commits using

```bash
git cherry-pick A..B
```

would reapply all commits from A (excluded) to B (included).

See also
- [Cherry pick](http://wiki.koha-community.org/wiki/Using_Git_Cherry_Pick)
- [How od you merge selective files with git merge](https://stackoverflow.com/questions/449541/how-do-you-merge-selective-files-with-git-merge/449632#449632)
- [How to cherry-pick multiple commits](https://stackoverflow.com/q/1670970/3337196)

### Patch

A patch can be generated...

```bash
git checkout -b branch
...
git commit -m "one of many? commits"
git format-patch master --stdout > my_branch_patch.txt
```

An applied somewhere else

```bash
git apply --stat my_branch_patch.txt    # shows the diffs
git apply --check my_branch_patch.txt   # tests for conflicts
git am --signoff < my_branch_patch.txt  # applies the patch
```

Alternatively one can also create a patch like

```bash
git format-patch HEAD~~                                   # will create two files for the previous two commits
git format-patch 338bf1bd431a34cd8d5f0a0cad524e2848b5cc98 # will create N files for the N commits since the marked commit
git diff 338bf1b fcc8a9f > my_patch.txt                   # Create a patch from the difference between two commits
```

See also
- [How to create and apply a patch with git](https://ariejan.net/2009/10/26/how-to-create-and-apply-a-patch-with-git/)
- [How to create and apply patches](http://makandracards.com/makandra/2521-git-how-to-create-and-apply-patches)
- [Git patch create and apply](http://www.thegeekstuff.com/2014/03/git-patch-create-and-apply/)
- [format-patch](http://git-scm.com/docs/git-format-patch)
- [Patching](http://git-scm.com/book/en/v2/Git-Commands-Patching)

and for `git am` vs `git apply` see also
- [git-am](http://git-scm.com/docs/git-am)
- [What is the difference between git am and git apply](https://stackoverflow.com/questions/12240154/what-is-the-difference-between-git-am-and-git-apply)

Conflicts
- [clone]

      git format-patch origin

- [orig]

      git am -3 --signoff 0001-my-commit-message.patch

  the -3 attempts to find a 3-way merge, and then allow to run

      git mergetools 

  if a conflict occurs

but it is not working if the original does not exist when the conflict is resolved

    git am --continue # as indicated

If a series of patches was generated, and we want to apply all of them but keeping each their own commit

    git am *.patch

See [how to apply multiple git patches in one shot](https://stackoverflow.com/questions/18494750/how-to-apply-multiple-git-patches-in-one-shot).

Sometimes, when applying a patch, there are errors, because the target files have been changed in compare to what the patch was expecting. This can be illustrated using

    git apply --reject --whitespace=fix file.patch

any file error would appear as `filename.rej`. One can use `wiggle` to try to force applying the patch as

    wiggle --replace file file.rej

Once all the errors have been fixed

    git status
    git add modified_files
    git am --continue

Further information on
- [Email](http://git-scm.com/book/en/v2/Git-Commands-Email)
- [Maintaining a project: git am](http://git-scm.com/book/en/v2/Distributed-Git-Maintaining-a-Project#_git_am)
- [git-am](http://git-scm.com/docs/git-am)

Exclude 
If a file from a patch would cause issues, it is also possible to exclude it:

    git apply 0001-foo.patch --exclude=conflicting.file


### Hooks


`hooks` are scripts that can be run when some `git` commands are run. This allows, e.g., to enforce some policy on push. But it can also force a formatting in commit messaged or further tasks.

Some information can be found on
- [Git hooks](http://www.git-scm.com/book/en/v2/Customizing-Git-Git-Hooks)
- [Enforced policy](http://www.git-scm.com/book/en/v2/Customizing-Git-An-Example-Git-Enforced-Policy)

Examples

- pre-commit

  ```bash
  #!/bin/bash
  # Generates a random number that is included into a header, and will be commited

  id="$(cat /dev/urandom | tr -dc '0-9a-f' | fold -w 8 | head -n 1)"

  file=version.h

  if [ -e $file ]
  then
      rm -f $file
  fi

  echo "#define GIT_VERSION 0x$id" >> $file

  git add $file

  exit 0
  ```

- prepare-commit-msg

  ```bash
  #!/bin/bash
  # Get the randomly generated ID from the file and add it to the commit message

  ID="$(sed -n 's/^.*\ 0x\([0-9a-f]\+\)/\1/p' version.h)"

  echo "[$ID]" >> $1

  exit 0
  ```

- post-commit

  ```bash
  #!/bin/bash
  # Write the git hash into a file. That file is not included in the commit

  file=version.h

  if [ -e $file ]
  then
    rm -f $file
  fi

  echo "#define GIT_VERSION \"$(git rev-parse --short=8 HEAD)\"" >> $file

  exit 0
  ```

- pre-push

  ```bash
  #!/bin/sh
  # prevents you from pushing a local local/bla local_jkdfs branch

  remote="$1"
  url="$2"

  z40=0000000000000000000000000000000000000000

  while read local_ref local_sha remote_ref remote_sha
  do
      if [  ! -z `echo "$local_ref" | grep "local*"` ]
      then
          echo "This is defined as a local branch."
          echo "A hook is prevening you from pushing local branches."
          exit 1
      fi
  done

  exit 0
  ```

- post-checkout

  is called by

      git checkout
      git checkout -b
      git clone

- post-merge

  is called by

      git merge
      git pull

Include git hash into a file

```bash
cat .git/hooks/post-commit
    #!/bin/sh

    echo "Running post-commit"
    ./update_git.sh

cat .git/hooks/post-checkout
    #!/bin/sh

    echo "Running post-checkout"
    ./update_git.sh

cat update_git.sh
    #!/bin/sh

    FILE=version.h

    if [ -e $FILE ]
    then
        rm -f $FILE
    fi

    echo "#define GIT_VERSION \"$(git rev-parse --short=8 HEAD)\"" >> $FILE

    echo "$FILE contains:"
    cat version.h
```

`version.h` gets updated on each commit/checkout but also with clone, when doing, e.g.

    git clone --template=path/to/original/.git path/to/original


### Notes

Reference: [Notes](https://git-scm.com/2010/08/25/notes.html).

Add a note

    git notes add <commit>
    git notes add -m "text of the note" <commit>

Edit a note

    git notes edit <commit>

Remove a note

    git notes remove <commit>

See notes

    git log --show-notes

It is possible to share the notes, but that can be somewhat complex and a source of problems.

### Changes requests

#### Request-pull

Prints out a summary of the changes between your branch and another branch.
This information can be sent to other users, to suggest to pull your changes into their system to review your code

See
- [Request pull](http://www.git-scm.com/docs/git-request-pull)
- [How to send pull request on git](https://stackoverflow.com/questions/6235379/how-to-send-pull-request-on-git)

```bash
git request-pull [options] start url [end]
git request-pull v1.0 https://git.ko.xz/project master
```

#### Merge requests

Some platform allows to do merge requests (e.g. github/gitlab). But it with a given link, it could be done by using

```bash
# Get the remote branch:
git fetch git@serveur:repo BRANCH_NAME

# Switch to branch locally
git checkout -b mr/feature1 FETCH_HEAD
```

The merge can then be handled locally.

### Philosophy and issues

#### Workflows

Read the following
- [Ref.](http://www.randyfay.com/node/89)
- [Git workflow](https://sandofsky.com/blog/git-workflow.html)
- [Workflows](https://www.atlassian.com/zh/git/workflows)
- [Maintaining a project](http://git-scm.com/book/en/v2/Distributed-Git-Maintaining-a-Project)
- [Gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow)

#### Fast-forward or not

to ff or not to ff
- [Fast forward](http://ariya.ofilabs.com/2013/09/fast-forward-git-merge.html)
- [Why does git fast forward merges by default](https://stackoverflow.com/questions/2850369/why-does-git-fast-forward-merges-by-default/2850413#2850413)

#### Commit early commit often

Arguments on why more commits is preferable.

Git:
- Decentralised: no single central points, so commits do not have to be shared with everyone all the time
- At the core of the tool
- Used with many commits by large and experienced communities (e.g. Linux kernel)
- Accomodates various workflows

Development:
- Keep trace of the steps of the development (the order of the things as well as the 
- Allows to run `bisect` tools
- Allows more frequent local merge of other developments
- Help design atomic elements
- Can prevent hardware (HDD) failures
- Allows review of alternative/past solutions

Read also
- [Commit Often, Perfect Later, Publish Once: Git Best Practices](https://sethrobertson.github.io/GitBestPractices/)
- [Git: Commit Early, Commit Often](http://www.databasically.com/2011/03/14/git-commit-early-commit-often/)

### Change history

#### Edit last commit

edit the last commit's comment

    git commit --amend 

It also allows for adding/removing files from the commit. Not to use when commit is pushed.

See [Edit an incorrect commit message](https://stackoverflow.com/questions/179123/edit-an-incorrect-commit-message-in-git)

e.g. one file forgotten/not complete

    git add file
    git commit --amend

#### Edit earlier commit

to come back 3 commits in the past.

    git rebase -i HEAD~3

See also [Rewriting History](http://www.git-scm.com/book/en/v2/Git-Tools-Rewriting-History).

To modify the history (are you sure?), one can do

    git rebase --interactive '##hash##^' # change pick to edit
    git commit --all --amend --no-edit   

`--no-edit` should be removed if one wants to rewrite the comment

    git rebase --continue

If the commits were already shared (pushed), that might create some issues and thus should be avoided.

See [How to modify a specified commit in git](https://stackoverflow.com/questions/1186535/how-to-modify-a-specified-commit-in-git#1186549)


#### Discard modifications

remove modifications since last commit

    git checkout -- filename

#### Merge correction

Forgot the no-ff:

```bash
git checkout master
git merge branch
git checkout HEAD~~    # come back two commit in the past: e.g. where it was before the merge
git merge --no-ff branch
git push origin master
```

#### Author name

Change the author name

```bash
git commit --amend --author="New Name <email@example.com>"
```

To change several commits at once, one can use

``` bash
# This part isn't technically required, but avoid further issues
git config --local user.name "New Name"
git config --local user.email "email@example.com"

# Rewrite history
git rebase -i HASH -x "git commit --amend --author 'New Name <email@example.com>' -CHEAD"
# Simply save the message that pops

# Re-publish (skip if it was only local)
git push --force origin HEAD
```

See also [How to amend several commits in Git to change author](https://stackoverflow.com/q/4981126/3337196).

### Removing a file from history

See [Completely remove file from all git repository](https://stackoverflow.com/questions/307828/completely-remove-file-from-all-git-repository-commit-history#308684)

It can be done as

```bash
git filter-branch --index-filter 'git rm --cached --ignore-unmatch <file>'
git for-each-ref --format="%(refname)" refs/original/ | xargs -n 1 git update-ref -d
git reflog expire --expire=now --all
git gc --aggressive --prune=now
```

### Split a repository

If we have a structure like

    foo/
        .git/
        abc/
        def/

and want to get to

    foo/
        .git/
        abc/
    bar/
        .git/
        def/

for a  recent git version (>=1.7.11)

    git subtree split -P def -b def_split_branch_name
    mkdir bar
    git init
    git pull foo def_split_branch_name
    git filter-branch --tree-filter 'rm -rf def' HEAD # cleans the history

See [Detach subdirectory into separate git repo](https://stackoverflow.com/questions/359424/detach-subdirectory-into-separate-git-repository/17864475#17864475)

for an older version (other answer on the same link)
- clone branch

      git clone foo bar

- preserve branches

      cd bar
      for i in $(git branch -r | sed "s/.*origin\///"); do git branch -t $i origin/$i; done
      git remote rm origin

- remove tags

      git tag -l | xargs git tag -d

- remove mention of other than def dir in logs

      git filter-branch --tag-name-filter cat --prune-empty --subdirectory-filter def -- --all

- clear & gc

      git reset --hard
      git for-each-ref --format="%(refname)" refs/original/ | xargs -n 1 git update-ref -d
      git reflog expire --expire=now --all
      git gc --aggressive --prune=now

- remove def from foo

      cd foo
      rm -r def
      git rm -r def
      git commit -m "def moved to its own directory"

One can also remove def from the history, but why not keeping trace of a shared history?

see also:
- [Split large git repository into many smaller ones](https://stackoverflow.com/questions/3910412/split-large-git-repository-into-many-smaller-ones)
- [Splitting and shrinking a git repository](https://home.regit.org/2010/08/splitting-and-shrinking-a-git-repository/)
- [Splitting a subfolder out into a new repository](https://help.github.com/articles/splitting-a-subfolder-out-into-a-new-repository/)
- [When to break up a large git repository into smaller ones](https://stackoverflow.com/questions/21941068/when-to-break-up-a-large-git-repository-into-smaller-ones)

### Merging two repositories


we have two repositories

    mainrep/
    repdir/

and want to get

    mainrep/
        repdir

- using subtree

      git subtree add --prefix=repdir /path/to/rep/repdir branch-to-merge-name
      pb. Not always available

- using submodules: see above. pb. Not really merging
- using simple merge

      git remote add repdir-rem /path/to/rep/repdir
      git pull repdir-rem branchname
      mkdir repdir
      git mv files-from-repdir repdir/files-from-repdir
      git commit -m "moving files to subdir"

  pb. not elegant / two steps. history of single files can be obtained by

      git log --follow repdir/myfile

  this is due to git mv doing git rm/git add the files would appear in

      git log --all --pretty=format: --name-only --diff-filter=D | sort -u # list all deleted files
      git log --diff-filter=D --summary # for some more information

- using subtree merge

      git remote add -f repdir-rem /path/to/rep/repdir
      git merge -s ours --no-commit repdir-rem/master
      git read-tree --prefix=repdir/ -u repdir-rem/master
      git commit -m "Merge repdir project as our subdirectory"
      git pull -s subtree repdir-rem master # future updates

  pb. a git log shows all information but git log repdir/myfile is broken

- using rebase: see discussions above. pb. repeating history, not merging it

See also:
- [How do you merge two git repositories](https://stackoverflow.com/questions/1425892/how-do-you-merge-two-git-repositories)
- [Subtree merging and your](http://nuclearsquid.com/writings/subtree-merging-and-you/)
- [About git subtree merges](https://help.github.com/articles/about-git-subtree-merges/)
- [How to import existing git repository into another](https://stackoverflow.com/questions/1683531/how-to-import-existing-git-repository-into-another)
- [Merge two git repositories without breaking file history](https://stackoverflow.com/questions/13040958/merge-two-git-repositories-without-breaking-file-history/)
- [Merging two git repositories into one repository without losing file history](http://saintgimp.org/2013/01/22/merging-two-git-repositories-into-one-repository-without-losing-file-history/)
- [How can I rewrite history so that all files are in a subdirectory](https://stackoverflow.com/questions/4042816/how-can-i-rewrite-history-so-that-all-files-are-in-a-subdirectory)

### Submodule merge into main repo


See the [SO reference](https://stackoverflow.com/questions/1759587/un-submodule-a-git-submodule).

with the main repo being `repo1` which includes a submodule: `subrepo1` (and one only)

1. Get the code

       git clone repo1
       git submodule init
       git submodule update

2. Get the history of the submodule into the main

       git remote add subm_origin subrepo1   # Adding as remote the address of the submodule
       git fetch subm_origin                   

       git merge -s ours --no-commit subm_origin/branch     

  to make sure of the name of the branch, one can run `git submodule` this shows the commit/branches of the submodule, one should use the `subm_origin/<last_name_after_slash_from_git_submodule_command>`.
  
  It is possible that git complains about unrelated histories, which is actually true. But then the option: `--allow-unrelated-histories` should allow to merge those.

3. Suppress references to the submodule, without deleting the code
   
       git rm --cached path/to/subm
       git rm .gitmodules
   
   alternatively
   
       vim .gitsubmodules
       git add .gitsubmodules
   
   finally
   
       rm -rf path/to/subm/.git

4. Add the code as new 
   
       git add path/to/subm
       git commit -m "submodule reintegrated"

5. Clean
   
       git remote rm subm_origin

Note that the last part (after the merge) integrate the current files into the current repo. But the merge and preceeding allows to keep the history of the submodules.



### Recover data and garbage collector

Calling the garbage collector

    git gc

See also [Maintainance and data recovery](http://www.git-scm.com/book/en/v2/Git-Internals-Maintenance-and-Data-Recovery).

### Tree visualisation


built-in

    git log --graph --pretty=oneline --all --abbrev-commit --decorate

one can set it as

    git config --global alias.tree "log --oneline --decorate --all --graph"

or

    git config --global alias.tree "log --pretty=\"format:%C(auto)%h %ad%d [%aN, %G?] - %s %N\" --all --decorate --graph --color --date=short"

and run

    git tree

extra command: `tig`

    aptitude install tig
    tig --all

(interactive/ncurses)
Use 'h' to get to some inline help

alternative: `gitk`

    aptitude install gitk
    gitk

(GUI)

### Configuration

Configuration can be done on a local scale (repo only) or global scale (all projects)

    git config --global user.name "Bilbopingouin"
    git config --global user.email "bilbopingouin@]

saves in `~/.gitconfig`

    git config user.name "Bilbopingouin" # in current repo

Useful commands may also be

    git config --global merge.tool vimdiff
    git config --global diff.tool vimdiff
    git config --global core.editor vim
    git config --global alias.tree "log --pretty=oneline --decorate --all --abbrev-commit --graph"
    git config --global merge.conflictstyle diff3
    git config --global branch.master.mergeoptions --no-ff --no-commit
    git config --global core.excludefile ~/.gitignore_global
    git config --global color.ui auto
    git config --global core.pager "less -FX"

See also [git contig](http://sip-router.org/wiki/git/gitconfig).

and using a global `.gitignore_global` as

    cat .gitignore_global
      # Windows
      .DS_Store

      # vim / backups
      .*.swp
      *~

      # archive
      *.tar.bz2
      *.tar.gz
      *.zip

See [Git configuration](http://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration).

When having personal repo in a work environment, it might be a good idea to have local config

    git config --local user.name "bob"
    git config --local user.email "bob@email.me"


### Migrate a git repo

Use the following

    git clone project@server1
    cd project
    git pull --all
    for remote in `git branch -r | grep -v master `; do git checkout --track $remote ; done
    git remote add server2 git@server2:project
    git push server2 --mirror

### Install latest git

There are various ways to achieve it.

- automatic:

      aptitude install git

- semi-automatic:

  see
  - [Installing latest version of git in Ubuntu](https://stackoverflow.com/questions/19109542/installing-latest-version-of-git-in-ubuntu/19109661#19109661)
  - [Ubuntu: git-core candidate](https://launchpad.net/~git-core/+archive/ubuntu/candidate)

- manual:

  - if git already there:

        git clone https://github.com/git/git

  - else:

        wget https://www.kernel.org/pub/software/scm/git/git-2.2.2.tar.xztar -xvJf git-2.2.2.tar.xz

    install some required packages

        aptitude install autoconf
        aptitude install asciidoc
        aptitude install docbook2x
        aptitude install curl

    build

        make configure
        ./configure --prefix=/usr/local
        make all doc info
        su -c "make install install-doc install-html install-info"

    See [Installing git](http://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

    make it available

        sudo echo -e "#\!/bin/bash\\n/usr/local/bin/git \$*" > /usr/local/bin/git-2.3
        sudo chmod +x /usr/local/bin/git-2.3

### Orphaned commit


when doing

    git commit --amend

or

    git rebase

some commits become unreachable. More importantly the sequence

    git commit -m "b"
    git commit -m "c"
    git checkout HEAD~ # back to b
    git commit -m "d"

`d` is a detached HEAD. Doing

    git checkout master

would bring the commit d unreachable.

git warns about such problems, but it can be solved by

    git checkout -b branch_name # before the checkout master

or

    git branch branch_name <HASH>

but then how to get the HASH if it is not references anymore. One ca run

    git fsck --unreachable

or

    git fsck --lost-found

which provide a list of all orphaned commits wanted or not ([List of all git commits](https://stackoverflow.com/questions/4786972/list-of-all-git-commits)).

another option is to run

    git reflog

which gives a history of all previous commands.

See
- [Recovering lost git commits](http://effectif.com/git/recovering-lost-git-commits)
- [What's the difference between git reflog and log](https://stackoverflow.com/questions/17857723/whats-the-difference-between-git-reflog-and-log)

I can use that to find the hash of a lost commit point to the branch which should be there and do a

    git reset --hard <HASH>

### Sync branches local/remote

List branches

    git branch -a   # lists all branches
    git branch -vv  # lists local branches indicating the tracking

Delete branches

    git branch -d my_branch             # deletes the local branch my_branch
    git push origin :my_branch          # deletes the remote branch my_branch
    git push origin --delete my_branch  # deletes the remote branch my_branch

Track remote branches

    git checkout --track origin/my_branch                   # creates a local branch which tracks the remote
    git checkout -b my_local_branch origin/my_remote_branch # the same but gives it a different name locally
    git branch -u origin/my_branch                          # sets your current branch to track the remote one

Update tracking

    git remote prune origin # remove local branches which track a remote that was deleted
    git fetch -p            # same

Update all local branches

    git fetch --all
    git remote update

See
- [Remote branches](http://www.git-scm.com/book/en/v2/Git-Branching-Remote-Branches)
- [How do you delete a remote branch properly](https://stackoverflow.com/questions/5183051/how-do-you-delete-a-remote-git-branch-properly-a-k-a-updating-the-remote-bra#5183258)
- [Differences between git remote update and fetch](https://stackoverflow.com/questions/1856499/differences-between-git-remote-update-and-fetch)

### Prevent from pushing a local branch


In `.git/hooks/pre-push` we can write

```bash
#!/bin/sh

remote="$1"
url="$2"

while read local_ref local_sha remote_ref remote_sha
do
if [  ! -z `echo "$local_ref" | grep "local*"` ]
then
    echo "This is defined as a local branch."
    echo "A hook is prevening you from pushing local branches."
    exit 1
fi
done

exit 0
```

That way, any branch within that local repository named `local` or `local/*` will be blocked when calling `git push`.

- [Block a git branch from being pushed](https://stackoverflow.com/questions/6644329/block-a-git-branch-from-being-pushed)
- [Writing a git post-receive hook to deal with a specific branch](https://stackoverflow.com/questions/7351551/writing-a-git-post-receive-hook-to-deal-with-a-specific-branch)

See also how to get the hook as a [global setting](https://stackoverflow.com/questions/2293498/git-commit-hooks-global-settings).

### Change origin url


Test remote url:

    git remote show origin

Add a new url

    git remote add server git@gitserver:prj

Change/Update the url

    git remote set-url origin git@gitserver:prj

### Signed commits


Reference: [Signing your work](https://git-scm.com/book/en/v2/Git-Tools-Signing-Your-Work)

Generate a key

    gpg --gen-key

Configuration

    git config --global user.signkey 12345678

Signed commit

    git -S -am "Commit"

Read signatures

    git log --show-signature
    git log --pretty="format:%h %G?" 

`%G?` provides a `G` when signed and a `N` when not signed.

Sign tag

    git tag -s v1.5 -m "my signed tag"

using -s instead of -a

### End-of-line


When collaborating with users on different computer OSes, one might avoid issues with CRLF (Carriage Return Line Feed).

References:
- [How line ending conversions work with git core.autocrlf](http://stackoverflow.com/questions/3206843/how-line-ending-conversions-work-with-git-core-autocrlf-between-different-operat#4425433)
- [Dealing with line endings](https://help.github.com/articles/dealing-with-line-endings/)

On Windows, one should run

    git config --global core.autocrlf true

On OSX or Linux, better

    git config --global core.autocrlf input

But some other options can be considered as indicated in the links above.

### Merge taking always the remote

Running

    git merge --strategy-option theirs

See [How to keep the local file or the remote file during merge using git](https://stackoverflow.com/questions/6650215/how-to-keep-the-local-file-or-the-remote-file-during-merge-using-git-and-the-com).


### Merge single file / partial merge

There are various alternatives to do it. In particular to keep a nice history tree.

Checking out the file from another branch

```bash
git checkout branch file1 [file2 ...]
git commit -m "merge file1 from branch"
```

it should be noted that it somehow breaks the history (it is no merge, works more like a patch, which could also be an option). See 
[How to merge specific files from another branch](http://jasonrudolph.com/blog/2009/02/25/git-tip-how-to-merge-specific-files-from-another-branch/)
This could still be marked by using

    git merge branch -s ours 

which will create a new commit, but without changing anything to the content.

Another way is to run

    git merge --no-ff --no-commit branch

Reject updates by

    git checkout HEAD^ file

or

    git reset HEAD^ file

but then git consider the merge to be full and you cannot re-run merge to add the missing files ([How do you merge selective files with git merge](https://stackoverflow.com/questions/449541/how-do-you-merge-selective-files-with-git-merge/7292109#7292109)).

An alternative to achieve the same effect

```bash
git merge --no-ff --no-commit -s ours branch        # -s ours reject all modification, but mark the parenting
git checkout branch -- file
```

See also [How do you merge selective files with git](https://stackoverflow.com/questions/449541/how-do-you-merge-selective-files-with-git-merge/12613427#12613427).

`git cherry-pick` and `git rebase -i` offer options when modifications on that file are in separate commits
(ditto)

```bash
git cherry-pick <commit hash>     # apply the commit to the current branch (automatically commit)
```

or even [split the commits](http://plasmasturm.org/log/530/).

another alternative is to use

    git checkout branch file
    git stash
    git merge stash

this gives some information that the version from file is merged from somewhere and can be followed by

    git merge branch

See also
- [How do you merge selective files with git merge](https://stackoverflow.com/questions/449541/how-do-you-merge-selective-files-with-git-merge/7184182#7184182)
- [Stashing](http://git-scm.com/book/en/v1/Git-Tools-Stashing)

### Git platforms

#### Gitolite: git on server

See
- [Git on the server: gitolite](http://www.git-scm.com/book/en/v1/Git-on-the-Server-Gitolite)
- [user rights](http://gitolite.com/gitolite/progit.html)
- [user view](http://gitolite.com/gitolite/user.html#pers)
- [wild repo](http://gitolite.com/gitolite/wild.html)

##### Wild repo

in gitolite conf:

    repo sd/user/CREATOR/[a-zA-Z0-9_\.\-].*
    C       =   @admin CREATOR
    RW+     =   @admin
    RW      =   CREATOR
    R       =   @all

as user

    git clone git@gitserver:sd/user/tlb/P0023_FC_SubSea-feas-energy
        
cloning a local repo:

    git remote add local ../P0023_FC_SubSea/
    git fetch --all
    git merge --ff local/master
    git push origin --mirror
            
or otherwise

    repo sd/user/CREATOR/[a-z]..*
        C   = @dev
        RW+ = CREATOR
        RW  = WRITERS
        R   = READERS
            
and then

    ssh git@gitserver perms -h

which allows to set the permissions to WRITERS and READERS (or decide who is part of those)

##### List of available repos

    ssh git@gitserver info # requires read rights for git on .gitolite.rc

##### Rename repo

    cd ~/repositories 
    mv old-name new-name

on the local, 

    vim conf/gitolite.conf

change the name of the repo

    git commit -am "changing old-name to new-name"
    git push origin master

##### Rights

- `-|R|RW+?C?D?M?`
- `-`       access deny
- `R`       read 
- `RW`      read, modify, add new branch, etc.
- `RW+`     read, modify, add new branch, etc. + rewind, delete branch
- `RW+C`    RW+ without ref Create (branch, tags)
- `RW+D`    RW+ without ref Delete (branch, tags)
- `RW+M`    RW+ without Merge

with the following

    RW master = bob

bob has RW rights but only on branch master

Note that the order is important

##### Remote commands

- `ssh git@gitserver help`          prints some help
- `ssh git@gitserver info`          lists the repo accessible
- `ssh git@gitserver perms`         sets permission for wild-repos
- `ssh git@gitserver desc`          shows or sets descriptions for repos
- `ssh git@gitserver writable`      enables or disables push to a repo
- `ssh git@gitserver xxxxxxxx -h`   provides help on the command

##### Multiple keys

Some user want to have an access from different machines

    keydir/alice/home/alice.pub
                /work/alice.pub
          /bobsmith/home/bob.pub
          /admin.pub

In the configuration file, `alice` can be used, and will match both keys.

##### Aliases/groups

It is possible to define groups and or aliases for users like

    @dev=alice bob

    repo proj
        RW+ = @dev

But also to give some default rights to a series of repos

    @productA=prodA/..*

    repo @productA
        RW+ = alice
        R   = bob

    repo prodA/proj1
        RW+ = bob

    repo prodA/proj2
        RW+ = carl

Alice and Bob have `RW+` rights on `proj1`
Alice and Carl have `RW+` rights on `proj2`, but Bob still has `R` rights.

#### Github

See [Github](https://github.com/)

#### GitLab

See [GitLab](https://about.gitlab.com/)

##### Merge requests

It is possible to get information about the merge requests locally. This can be done using

```bash
git config remote.origin.fetch '+refs/merge-requests/*:refs/remotes/origin/merge-requests/*'
```

And then runs `git fetch`. It gets the `origin/merge-requests/1` labels (similar to branches). That way, it is possible to check locally the precise merge request concerned by the code.

See also
- [GitLab Merge Requests](https://www.jvt.me/posts/2019/01/19/git-ref-gitlab-merge-requests/)
- [GitLab checkout merge requests locally](https://docs.gitlab.com/ee/user/project/merge_requests/#checkout-merge-requests-locally)

If a user creating a merge request allows it, it is possible to work on their branch together. This would be done by doing the following

```bash
git remote add user_fork git@gitlab.com:user/projectname 
git fetch user_fork
git checkout user_fork/branchname

# modify things and commit

git push user_fork branchname:branchname
```

Note the importance of the `:`. See also [Gitlab: allow collaboration](https://docs.gitlab.com/ee/user/project/merge_requests/allow_collaboration.html)

### Tig

References
- [Tig](https://jonas.github.io/tig)
- [Git-Tig](https://www.atlassian.com/blog/git/git-tig)
- [Tig manual](http://jonas.nitro.dk/tig/manual.html)

Install

    aptitude install tig

See the tree of history

    tig --all

See colored git blame

    tig blame file.c

Use tig as a pager

    git show | tig
    git log | tig

Commands from within tig

- views switching
  - `m`       Switch to main view.
  - `d`       Switch to diff view.
  - `l`       Switch to log view.
  - `p`       Switch to pager view.
  - `t`       Switch to (directory) tree view.
  - `f`       Switch to (file) blob view.
  - `g`       Switch to grep view.
  - `b`       Switch to blame view.
  - `r`       Switch to refs view.
  - `y`       Switch to stash view.
  - `h`       Switch to help view
  - `s`       Switch to status view
  - `c`       Switch to stage view

- navigation
  - `q`           Close view
  - `<CR>`        Open diff view, or similar actions
  - `k`/`<up>`    Previous commit/line
  - `j`/`<down>`  Next commit/line
  - `<tab>`       Switch to the next view

- various
  - `Q`       Close tig
  - `e`       Open file in editor

Example usage:

- Explore history
  - `git --all`           Opens
  - `<down>`              Until the desired commit
  - `t`                   Opens the tree view
  - `<enter>`             Shows the file as it was

- Status/add/reset
  - `s`                   Switch to status: git status
  - `u`                   git add
  - `C`                   git commit
  - `!`                   git reset HEAD

- Follow modifications to a single line

      tig blame file.c

  move to the wanted line and then `,` runs `git blame` as parenting ('<' allows to jump back to child)

Some configuration can be placed into a `~/.tigrc` file ([tigrc manual](https://jonas.github.io/tig/doc/tigrc.5.html)), like

    set main-view-id = display:true

### Statistics

There are various tools available to get some statistics from a git repository. One of them is [Gitstats](https://github.com/hoxu/gitstats). It can be installed on Debian simply as

```shell
aptitude install gitstats
```

The simples way to run it would be using

```shell
gitstats repo output
```

It will generate different stats which will be provided in the `output` directory. Some options can be set and are summarised in its manpage.
