# GitCheatSheet
## Git Cheat-Sheet For The Command Line


#### Working With Branches

To list all branches:

	git branch -a

To switch to a branch:

	git branch [your branch]

To create a new branch and switch to it:

	git checkout -b [new branch]

To delete a local branch (after it has been merged):

	git branch -d [your branch]

To list which branches have been merged into the current branch:

	git branch --merged

To list which branches have not been merged into the current branch:

	git branch --no-merged

To see which branches are tracking which remotes:

	git branch -vv

To change the remote that a branch is tracking:

	git branch [branch_name] -u [new_remote]/[branch_name]

To re-create a deleted branch (where sha1=last commit in the branch):

	git checkout -b [branch name] [sha1]

To rename the current branch:

	git branch -m [newname]


#### Using Diff

To see the differences between two branches:

	git diff [branch1]..[branch2]

To see the differences between two commits:

	git diff [hash1] [hash2]

To see a summary of what will be pushed:

	git diff --stat [remote]/[branch]

To see a full diff of what will be pushed

	git diff [remote]/[branch]

To see the differences between local (workspace) and fetched/remote branch:

	git diff [branch] [remote]/[branch]

To see the list of changed files between local (workspace) and fetched/remote branch:

	git diff --name-only [branch] [remote]/[branch]

To see the difference of a specific file between local (workspace) and fetched/remote branch:

	git diff [branch] [remote]/[branch] -- [filepath]


#### Committing

To commit all changes with a commit message:

	git commit -am "Commit message"

To checkout a previous commit into the working directory:

	git checkout [revision] .

To return to the latest commit after checking out an old one:

	git checkout [branch]

To checkout a previous commit into a new branch:

	git checkout -b [new branch] [revision]

To modify a commit with a new message or add a file:

	git [changes]
	git commit --amend

To move current changes in to the stash:

	git stash

To put the changes back from the stash:

	git stash apply


#### Merging

To fix a merge conflict by overwriting a file altogether:

	# relative to which branch is currently checked out
	# so if master is current branch, and merging in feature/my_feature
	# --theirs -> feature/my_feature
	# --ours -> master
	git checkout --theirs [file]
	git checkout --ours [file]

To merge a branch back into master:

	# do work in newbranch
	# commit changes to newbranch
	git checkout master
	git merge newbranch


#### Viewing Commit Logs

To see log of abbreviated stats for each commit:

	git log --stat

To see the log of diffs introduced in the last 2 commits:

	git log -p -2

To see the log as an ascii graph with branch and merge history with abbreviated hash and the subject:

	git log --pretty=format:"%h %s" --graph


#### Undoing Changes

To unstage a file:

	git reset HEAD file

To discard changes to a file in the working directory:

	git checkout -- [file]

To reset all changes to all files:

	git reset --hard

To (recursively) delete untracked files from the working tree:

	git clean

To restore everything back to the way it was prior to the last commit:

	git reset --soft HEAD^     # use --soft if you want to keep your changes
	git reset --hard HEAD^     # use --hard if you don't care about keeping the changes you made


#### Working With Remotes

To clone a repo into a custom folder:

	git clone [repo_url] [folder]

To fetch data from the server (branches, etc) without merging it in to the local workspace:

	git fetch [remote]

To fetch data from a remote and merge it in to the local workspace:

	git pull [remote]

To see information about remotes (servers):

	git remote -v

To see detailed information about a remote:

	git remote show [remote]

To add another (e.g., upstream) repo to your forked/cloned repo:

	git remote add upstream [upstream url]

To push a branch to the remote:

	git push [remote] [newbranch]

To delete a remote branch from remote:

	git push [remote] :[newbranch]

To clean-up / refresh your references/tracking to remote branches:

	git remote prune [remote]

To update/sync a fork with the upstream repo:

	git fetch [upstream]
	git checkout [branch]
	git merge [upstream]/[branch]
	git push [origin] [branch]

To 'import' a branch from upstream into a fork:

	git fetch [upstream]
	git checkout -b [branch] [upstream]/[branch]
	git push -u [origin] [branch] # -u is optional - to update the branch tracking to the [origin] remote


#### Tags

To list a tags in a repo:

	git tag

To create a tag:

	git tag -a [tagname] -m "description"

To create a tag after you've moved past a commit:

	git tag -a v1.2 "version 1.2" [hash]

To push a tag to the remote:

	git push [remote] [tagname]

To delete a tag:

	git tag -d [tag_name]
	git push --delete origin [tag_name]


#### Using Submodules

To add an existing repo as a submodule:

	git submodule add [repo_url]

To clone a project with submodules:

	clone the repo like normal
	change to folder where submodule is
	git submodule init
	git submodule update


#### Using Subtree

To add a git subtree (and squash commits):

	git subtree add --prefix [destination_folder] [repo_url] [branch] --squash

To pull a subtree:

	git subtree pull --prefix [destination_folder] [repo_url] [branch] --squash


#### Rebasing
(Apply all the changes from one branch to another)

**Note: Do not rebase commits that have been pushed to a public repo!!**

To rebase the commits in a branch onto master:

	git checkout newbranch
	git rebase origin/master

To undo a rebase:

	git reset --hard ORIG_HEAD

To squash commits (where 3 is the last X number of commits to squash:

	git rebase -i HEAD~3


#### Aliases

	unstage = reset HEAD --
	undo = checkout --
	changes = diff --name-status -r
	diffstat = diff --stat -r
	new = !sh -c 'git log $1@{1}..$1@{0} "$@"'
	last = log -1 --stat HEAD
	who = shortlog -s --
	co = checkout
	br = branch
	ct = commit
	st = status
	df = diff
	lc = log ORIG_HEAD.. --stat --no-merges
	lg = log --stat
	lp = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit --date=relative
	oldest-ancestor = !zsh -c 'diff --old-line-format='' --new-line-format='' <(git rev-list --first-parent "${1:-master}") <(git rev-list --first-parent "${2:-HEAD}") | head -1' -
	sta = "!f() { git subtree add --prefix $2 $1 master --squash; }; f"
	stp = "!f() { git subtree pull --prefix $2 $1 master --squash; }; f"


#### Git SVN

Notes:

	1. Branch tracking is permanent. i.e., local master will always track SVN trunk, etc. and cannot be changed.


To clone an SVN repository:

	git svn clone [url] -T trunk -b branches -t tags [--prefix="origin/"]

To port the SVN ignore file to gitignore:

	git svn show-ignore > .gitignore

To port the SVN ignore file to git excludes:

	git svn show-ignore >> .git/info/exclude

To fetch the latest changes from SVN (but not rebase):

	git svn fetch

To fetch the latest changes from SVN and rebase local changes on top of latest in SVN:

	git svn rebase

To track an SVN branch (on the server):

	git branch [branch_name] remotes/origin/[branch_name]

To create a new branch on the SVN server (equivalent to "svn copy trunk branches/[newbranch]""):

	git svn branch [newbranch]

To push commits back to SVN:

	git svn dcommit

###### Git SVN setup:

1. Clone repo
2. Port the ignore file

###### Git SVN workflow:

1. Checkout master (trunk)
2. Fetch/rebase updates from SVN
3. Create new local branch and switch to it
4. Make changes and commits as normal for git (git add, git commit)
5. Checkout master branch and fetch/rebase latest changes from SVN
6. Merge or rebase local branch with/onto master (trunk). Either:
	1. (Preferred) Rebase local branch on to master
		* git checkout [branch]
		* git rebase master
		* git checkout master
		* git merge --ff-only [branch]
	2. (Alternate) Merge local branch into master and squash commits, specifying a merge commit message (-m "msg")
		* git checkout master
		* git merge [branch] --squash -m "Message"
7. Dcommit to push to SVN
8. Delete local branch
