# Introduction

- Git provides many ways to do the same thing

- what works depends on how your project uses Git

- however it doesn't hurt to know what alternatives are out there so you're not
  totally shocked if some other project does something different

- if you want to learn what any particular command does `git help` will get you
  started with that

# Know what goes into commit

- `git status` is a good starting point to know what what goes into commit

- examine the output carefully, it's actually very helpful and gives you
  instructions about what to do

## Untracked files

- there are two kinds of red lines, first you are going to encounter in a fresh
  repository are untracked files

- pay close attention to untracked files you have, if there's anything there
  that you won't want to add into repository, consider using `.gitignore` in
  order to hide them

- if you remove an untracked file, Git won't be able to restore it

- using `git add` will add them straight to stagin area

## Changes to be commited

- second red lines that you are going to encounter are under header changes not
  staged for commit 

- these changes are shown if you run `git diff` without any parameters

- if you want to remove changes made into a file, you can use `git restore <file>`

- `git add <file>` will move the files to staging area

## Staging area

- green lines indicate what's included in so called staging area and are going
  to be included in commit

- `git diff --cached` can be used to diff between staging area and last commit

- if you made a commit, you can review it by running `git show`

## Formatting commit messages properly

- Git commits can be e-mailed, which is why there are certain formatting rules
  that should be followed

- good editors and IDE:s will helpfully show errors in formatting

- first line should be short 50 characters or less summary of the change

- second line must be always left empty, some tools will get very confused if
  it's not empty

# Writing good commit messages

- good commit message describes the change clearly enough that it can be used
  as is in a pull request

- first line should describe what is being done, rest of the lines should tell
  why and context about the chance

- if commit is a fix, describe what was broken and what you did in order to fix it

- if it's an enhancement, describe what it improves

- some day, good commit message will save your commit from a revert

- also make sure that commit message and diff agree

http://turnoff.us/geek/commit-messages/
http://turnoff.us/geek/when-ai-meets-git/
https://xkcd.com/1296/

# Tags

- tags work bit like bookmarks

- most common use for them is to tag releases

- tags are tied to a commit, which means if the commit hash changes, tag is "lost"

- tags are not pushed by default, so you can have your own personal tags

# Branches

- when you need to work on multiple things at the same time, branches are handy
  way to accomplish that

- branches work bit like tree branches as they branch from the trunk, but
  what's really different that they quite often eventually merge back to the
  trunk, which makes situation look more like metrolines than a tree

# Merge VS rebase

- common debate is when to use merge and when to use rebase

- both have their uses and even heavy rebase user might have occasional use for
  merge now and then

- merge is pretty much only option if history has to be preserved accurately

- rebase on the other hand by definition rewrites history and repository with
  only rebase users has history as one straight line with no metrolines

# Keeping your branch up to date

- when working on a feature branch for long time, it will slowly drift apart
  from other development

- further it drifts away from master (or branch it's supposed to merged into),
  the eventual merge conflict is going to get harder and harder

- start always with `git fetch` as that will refresh remote branches 

- upstream merge branch is usually origin/master

- with merge process is to do `git merge <upstream merge branch>`

- that will result in messy, but very accurate history, but that's usual with
  merge

- with rebase, `git rebase <upstream merge branch>` will do the same thing, but
  will keep the history cleaner

# Cleaning up your history

- especially with rebase, you can get into situation where you need to resolve
  the same conflicts over and over again

- cleaning up your history will make resolving the conflicts bit easier later on

## Quick way

- fastest way to clean up the history, is reconstructing the commits

- if you want to reuse commit messages, you should copy the commit hashes to
  text file somewhere 

- first part of the process is to use `git reset` with first commit with
  changes you don't want to reconstruct

- that will remove the commits and leave the changes in staging area

- now you can add things one by one and make as many or few new commits as you
  like 

- when making commits, you can use `git commit -c <commit hash>` with one your
  old commits, so you don't have to rewrite the commit messages

- if you need to modify the commit message, you can edit the latest commit
  with `git commit --amend`

## More finegrained way

- key for this is `git rebase -i` (you usually want to give origin/master as
  parameter)

- that will let you remove, reorder or combine the commits

- way to prepare for it is to use `--fixup` parameter when making commits as
  that way rebase knows already what you are intending to do with the commit

- when doing the interactive rebase, you're supposed to reorder the commits in
  a way that fixup commits are next to the commits they are combined to

- faster way is to use the `--autosquash` parameter with the rebase and Git
  will reorder things automatically

- remember that you don't need to do rebase if you want to modify the latest
  commit, `git commit --amend` is lot handier for that

# Getting back to where you started

- there are situations where you want to just start over

- if you want to reset the situation to any commit, you can use 
  `git reset --hard commit`

- `git reset` doesn't care what branch commit belongs to

- there are to handy shortcuts, `HEAD` will refer to latest commit in the
  current branch, `origin/master` is last known state of master branch in the
  default remote

# Recovering from accidents

- most of the necessary building blocks were already mentioned

- `git log -g` shows low level log of the repository, so if you accidentally
  got rid of commit, you can still see it there and can use `git reset` to get
  that back

- if you get into merge conflict, you can get out of that using `--abort`
  command line option

- `--abort` works with several other commands, remember to use it with same
  command that caused the conflict

# Handling merge conflicts

- eventually you'll get into merge conflict that you just can't abort

- it's perfectly doable to resolve merge conflict by hand without external
  tool, though external tools (or IDE integration) can have more user friendly
  experience

- high level overview is that what you are trying to do is to merge two sides
  of the conflict into result that still makes sense after the conflict is
  resolved

- `git status` and `git diff` will give good overview of the situation

-  if you know that only other side is "right", you can use `git restore --ours
   <file>` or `git restore --theirs <file>` to get file from either side of the
   conflict (just remember that with rebase, the roles are reversed)

- when resolving conflict by hand, note the `<<<`, `===` and `>>>` lines as
  those are added by Git (they are used to indicate both sides of the conflict)

