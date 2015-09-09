# GitCheatSheet
##Git Cheat-Sheet For The Command Line##

To see a summary of what will be pushed:

	git diff --stat [remote]/[branch]

To see a full diff of what will be pushed

	git diff [remote]/[branch]

To see log of abbreviated stats for each commit:

	git log --stat

To see the log of diffs introduced in the last 2 commits:

	git log -p -2

To see the log as an ascii graph with branch and merge history with abbreviated hash and the subject:

	git log --pretty=format:"%h %s" --graph

To modify a commit with a new message or add a file:

	git [changes]
	git commit --amend

To unstage a file:

	git reset HEAD file

To reset all changes to all files:

	git reset --hard
	
To restore everything back to the way it was prior to the last commit, we need to reset to the commit before HEAD:

	git reset --soft HEAD^     # use --soft if you want to keep your changes
	git reset --hard HEAD^     # use --hard if you don't care about keeping the changes you made
	
To move current changes in to the stash:

	git stash

To put the changes back from the stash:

	git stash apply

To discard changes to a file in the working directory:

	git checkout -- [file]

To checkout a previous commit into the working directory:

	git checkout [revision] .

To return to the latest commit after checking out an old one:

	git checkout [branch]

To checkout a previous commit into a new branch:

	git checkout -b [new branch] [revision]

To see information about remotes (servers):

	git remote -v

To see detailed information about a remote:

	git remote show [remote]

To add the upstream repo to your forked/cloned repo:

	git remote add upstream [upstream url]

To clone a repo into a custom folder:

	git clone [repo_url] [folder]

To fetch data from the server (branches, etc) without merging it in:

	git fetch [remote]

To list a tags in a repo:

	git tag
	
To create a tag:

	git tag -a [tagname] -m "description"
	
To create a tag after you've moved past a commit:

	git tag -a v1.2 "version 1.2" [hash]

To push a tag to the remote:

	git push [remote] [tagname]

To list all branches:

	git branch -a
	
To create a new branch and switch to it:

	git checkout -b [newbranch]

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
	
To delete a local branch (after it has been merged):

	git branch -d [new branch]
	
To list which branches have been merged into the current branch:

	git branch --merged

To list which branches have not been merged into the current branch:

	git branch --no-merged

To push a branch to the remote:

	git push [remote] [newbranch]
	
To delete a remote branch from remote:

	git push [remote] :[newbranch]

To clean-up / refresh your references to remote branches:

	git remote prune [remote]

To re-create a deleted branch (where sha1=last commit in the branch):

	git checkout -b [branchname] [sha1]

To see the differences between two branches:

	git diff [branch1]..[branch2]

To see the differences between two commits:

	git diff [hash1] [hash2]

To see the differences between local (workspace) and fetched/remote branch:

	git diff [branch] [remote]/[branch]

To see the list of changed files between local (workspace) and fetched/remote branch:

	git diff --name-only [branch] [remote]/[branch]

To see the difference of a specific file between local (workspace) and fetched/remote branch:

	git diff [branch] [remote]/[branch] -- [filepath]

To update/sync my fork with upstream:

	git fetch [upstream]
	git co [branch]
	git merge [upstream]/[branch]
	git push [origin] [branch]

To see which branches are tracking which remotes:

	git branch -vv

To change the remote that a branch is tracking:

	git branch [branch_name] -u [new_remote]/[branch_name]

To 'import' a branch from upstream into a fork:

	git fetch [upstream]
	git co -b [branch] [upstream]/[branch]
	git push -u [origin] [branch]

To add an existing repo as a submodule:

	git submodule add [repo_url]

To clone a project with submodules:

	clone the repo like normal
	change to folder where submodule is
	git submodule init
	git submodule update

Aliases:

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
