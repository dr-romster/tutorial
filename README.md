# tutorial
#####Lynda.com GitHub tutorial


git stores configs at three levels
- system 	`/gitconfig`
- user  	`~/.gitconfig`
- project	 `my_project/.git/config`
```
git config --system
git config --global
git config
```
`git config -- user.name " "   ` - sets user name etc
`git config -- user.email " " `

`git config --list `  # lists all current global settings

to view the contents of the git file, go to home directory and view the .gitconfig file using cat
```
cd ~
cat .gitconfig
```
to change the text editor to Textmate or nano
```
git config --global core.editor "mate -wl1"  
git confif --global core.editor "nano"
```
to change colours when outputting its messages, so its not all monochrome
```
git config --global color.ui true
```
`git help log` # git manual

initializing git for a project once you've navigated to the target folder with cd
```
git init
```
`git add .  ` - gather all changes and add them to the staging index
`git -m " " ` - commit message in quotes - short single line + blank line + more complete description 
 best practice to have them 50 chr for first line, <72 for other

ticket tracking numbers or conventions are useful

commit log can be obtained by 
```
git log
```
`git log -n  1`   # limits the number of commits to view to x
```
git log --since=2018-02-01  
git log --until
git log --author" "
```
`git log --grep=" "` - any text that matches what's in the quotes (global regular expression)

#####three tree architecture

- working and respoistory
- staging index - make chagnes in staggered, change steps. Usfeul when you are making multiple changes / steps to multiple files

Git uses checksums for each change set based on the changes committed
the algorithm is __SHA-1__ hash and produces a 40 chr hash value

the _HEAD_ which is a 'pointer'  in the repository
always points to the current tip of the repository, points to the parent of the next commit, where writing commits take place.
_HEAD_ moves to currently checkout branch of the repository
```
cd .git
cat HEAD
cd refs 
cd heads
```
`cat master` - the current hash of the commit

`git status` - tells us our status of the three branches and where the head is pointing,
to add new files to git tracking use git add 

#####to compare changes between what is in the repository and the working directoy
```git diff
git diff --staged 
```
__to look at changes between reposiroty and staging directory. once comiited this clears__

`git rm` # to remove files ready for commit

renaming files in git uses `git mv`
```
git mv second_file.txt secondary_file.txt
```
`git diff --color-words` - displays the differences next to each other in different colours

when viewing edits in `git diff` mode, to wrap/fold text lines use  - then Shift+S

`git -am ' ' ` - go straight to commit without having to go through the staging / git add bit

restoring repository versions

`git checkout` - gets the named element (file or branch) and make that look like your working directoty

a (`--`) double dash keeps git in the current branch
```
git checkout -- index.html
```
unstaging from the staging area
`git reset HEAD resources.html`

#####Undoing commits 
- we can only change the last commit (the one that HEAD points do) not furtherback as this breaks the _SHA-1_ hash chain
```
git commit --ammend -m 'message'

git checkout (SHA-1 hash) -- file
```
`git revert (SHA-1 hash) ` # undoes the changes totally from  a previous commit, if add -n then it will only stage the revert

if it needs revert something complex you will essentially be merging your previous commands with a new set of blah

`git reset` - allow us to undo many commits by specifying where the HEAD pointer is directed - options are 
	--soft 
		does not change the staging index or working directoty
	--mixed # default
		changes the staging index to match repository
		does not change the wd
	--hard
		changes the staging index wd and repository back to a previous reversion - bad news!
```
git .cat/HEAD
```
`git reset HEAD --soft (hash) ` - where hash is a previous commit hash version and puts it in the staging area

the old commits are still there after a hard reset if you still have the later hash numbers using 
`git reset HEAD (hash)`

#####Removing files from working directory

`git clean -n ` - tells you as a trial run what will be removed but does not remove them - just tells you what

`git clean -f` - destrucively, forced removal

#####Auto ignore files
`.gitignore`
	this will take * ? [aeiou] [0-9] and negate expressions with !
	ignore all files in directory using a trailing #
	it will take comments with a #
creating the .gitignore in terminal can be done with nano
`nano .gitignore`
to just view the .gitignore you can use `cat`
`cat .gitignore`

Useful things to go in a .gitingore file are:
- compiled source code, zip/.gz
- logs / OS created files
- iso / dmg files
- user uploaded content like videos / pdfs / images

user specific ignores
`git config --global core.excludesfile ~/.gitignore_global`
where `.gitignore_global` is the user specific `.gitignore` file for all repositories
useful when people are working on different platforms

to remove file from the staging area to allow you to do a staged commit
`git rm --cached`

#####tracking an empty directory with git
git only tracks files, so to do this you put a small file in it which is by convention either .gitignore or .gitkeep

you can do this from the command line using `touch` which adds an empty named file to the target directory

`touch assets/pdfs/.gitkeep`

#####Exploring the tree
tree-ish
	use the full or partial component of a hash to reference a commit
	the HEAD pointer 

parent commit
	-HEAD^, -hash^, master^
grandparent commits
	-HEAD^^ or -HEAD~2 or master^^ etc
greatgrandparent commits
	-HEAD^^^ or -HEAD~3 or hashes

`git ls-tree {treeish}`

expanding on the git log
to see log in single line form
`git log --oneline ` [also takes --since, until  -n for last few ]
`git log --since="2012-06-20" --until="3 days" ` (or 3.days -wihtout "")
```
git log -5
git log --author=" "
git log --grep='Temp'
```
`git log 2907d12...acf898974 `(from SHA-1 (1) until SHA-1 (2))
`git log 0demfdf .. index.html` (what affects particular file from a SHA-1)
`git log -p (sha-1) (file.html)` - will list all the changes to the file
`git log --stat `(what changed in each one)
`git log --summary` (can be combined with --stat)
`git log --format=oneline `(gives the full SHA in oneline)
`git log --graph `(shows branches and merges) combines well with --oneline --decorate

`git show (sha) - shows the diff, also takes --format=`

#####comparing commits
`git diff (sha) ` - compares current working directory with previous point in time
```
git diff (sha) (Filename)
git diff (sha)..(sha2)
git diff (sha)..(sha2) (filename)
git diff (sha)..HEAD^  - or any other treeish
git diff --stat --summary (sha)..HEAD -b or -w
```
 (-b wil ignore whitespace changes, -w will ignore all space change, 
equivalent to --ignore_all_space)

#####BRANCHING

Cheap in terms of memory and space
good for isolating features to enable collaboration

one working directory
the HEAD always points to the last commit in the master branch
once you branch it moves to the branch until you move back to the master or make changes to the master branch

`git branch` (shows available branches, current branch denoted by *)

to find the head: cat .git/HEAD

`git branch {new_branch_name}`

to move to a new branch 
```
git checkout new_feature
git checkout master
```
`git checkout -b (new_branch_name)` - `-b` makes a new branch and moves you to it in one step
you can't move to a new branch until you have committed changes to a branch

comparing branches

```
git diff (branch 1)..(new_branch)
```
using the `--color-words`  option will allow you to see the changes side by side in different colours

`git branch --merged`

deleting branches

`git branch -m new_feature seo_title`

`git branch -d (branch_to_delete)` - can force a delete with -D if you have not merged it yet

`export PS1='\W$(__git_ps1 "($s)") > ' `
- this will configure the prompt to show the working directory and current branch in brackets which is pretty handy, if this line is already in your ~/.bash_profile then to obtain this prompt when you start terminal type:

`source ~/.bash_profile `

#####to merge 
make sure you are on master branch

```
git checkout master

git merge seo_title
```
always have a clean working directory before merging otherwise it makes life complicated later when you want to unwind things (git status)

to see what has been merged

`git branch --merged`

and then to remove a merged branch

`git -d `

Fast forward merges
if the head is in the ancestry of the the master branch then it can merge by fast forwarding along the chain. This implies that no changes have occurred to the master chain whilst the branch was being edited

`git merge --no-ff branch`  (forces git to make a new commit with a new SHA)

`git merge --ff-only branch` (only merge if you can do it ff)


#####Merge Conflicts
Two changes in the same line in different branches
three choices:
1. 	abort merge  	 `git merge --abort`
2. 	manual resolution  
3.	merge tools

manually edit the conflicted files
conflicting areas demarcated by < <<<<<<HEAD > < ======= > and < >>>>> >
make your changes to one of them, delete these added bits
add the changed file to git and commit them
you won't need to add a message and the commit will complete the merge

#####Good practice tips
1.	keep lines short
2.	keep commits small and focused
3.	beware of stary edits to whitesapce 
		space / tab / line returns or else you will get merge conflicts
4.	merge often
		less likely to precipitate conflicts
		
You can continue to edit branches even after merging them

If the master is changing lots, merge master back into the branch or else you'll get too far out of sync. 'tracking' e.g.

m -- m --  m -- m --m --m -- m -- m
	\		\	        /
	  b -- b --b -- b --b -- b -- b --b

#####The stash

This is a special 4th zone which are similar to commits but are not assigned a
_SHA_

Useful for saving quick changes when moving back to master following changes that you don't want to commit them yet
```
git stash save "{message}"
git stash save " " --include_untracked
````
to inspect what is in the stash

`git stash list`  - gives you a reference and which branch it is stashed to

`git stash show -p stash@{0} ` - the -p shows the change details as a patch like a diff

git will try and bring stashes into your working directory, but this can generate conflicts

#####Pulling out stash
`git stash pop {stash ref}`		- also removes it from the stash - defaults to first item if not specified
`git stash apply	{stash_ref}	`- leaves a copy in the stash e.g. if you want to apply to multiple branches

Deleting stash

`git stash drop {stash ref}
git stash clear  all items in the stash`

####Remotes

Push and fetch
origin/master branch mirrors the remote server

`git remote {alias} {url}`
`git remote -v` (gives details of remote repository)

`git push -u  {alias} {branch} `- -u enables remote tracking of the origin /master branches to make tracking easiier

`git branch -r  ` shows remote branches

cloning a repository

`git clone {url} {new_repository_name}`

cloned repositories are automatically remotely tracked (like `-u`)
you can select which branch you want using `-b`

cat .git/config wil tell you this is the case under the [remote] heading


