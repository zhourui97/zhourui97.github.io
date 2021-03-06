---
layout: page
title:	
category: blog
description: 
---
# Preface

	#commit log
	git log

## sepecify branch

	git log [repo/]branch
	git log ^banch1 branch2 #show commits that are not in branch1 but in branch2

Remember that your clone of the repository will update its state of any remote branches only by doing git fetch. You can't connect directly to the server to check the log there, what you do is download the state of the server with git fetch and then locally see the log of the remote branches.

Perhaps another useful command could be:

	git log HEAD..remote/branch

which will show you the commits that are in the remote branch, but not in your current branch (HEAD).

## show merges

	git log --merges //Print only merge commits. This is exactly the same as --min-parents
	git log --no-merges //Do not print commits with more than one parent. This is exactly the same as --max-parents=1.

## --not

	git log <branch> --not <branch2> //show commits of <branch> that not in <branch2>(since <branch2>), same as git log branch ^branch2

## show reflog

	git log -g

## limit

	#last 2 comit log
	git log -2

### --since(--after)

	--since=2.weeks
	--since='2 years ago'

### other

	--since, --after
	--until, --before
	--auther='hilojack'
	--committer
	--no-merges
	--all-match 

##	diff info 

	#'-p' general diff patch
	git log -p -2
	#'--stat' general diffstat
	git log --stat -2

## list file
like svn log -v

	git log -1 --name-only
	git log -1 --name-status
	git log -1 --stat

## format(--format)

	--format=oneline|short|full|fuller
	--format="%h - %an,$ar"
		%H commit hash
		%h abbreviation commit hash
		%T tree hash
		%t abbreviated tree hash
		%P parent hash
		%p abbreviated parent hash
		%an author name
		%ae author email
		%ad author date
		%ar author date(relative)
		%cn committer name(relative)
		%s subject 

### other

	--graph //brach log info
	--shortstat
	--name-only
	--name-status //file list
	--abbrev-commit
	--relative-date
	--pretty //same as --format

## merge log
-m:

	git log -m -1

## log context

	-U<n>, --unified=<n>
           Generate diffs with <n> lines of context instead of the usual three


# shorlog

	git shortlog [<object>] 
	git shortlog <object> --not <object>

# git reflog
引用日志：HEAD指向的日志
	
	git reflog //same as `git log -g`
	c19cfa7 HEAD@{0}: commit: add css:blockquote
	64c86f6 HEAD@{1}: commit: add a line
	git show HEAD@{2}

# git show
show info about <object>

	git show <object> //U could specify a tag or a commit as an object

