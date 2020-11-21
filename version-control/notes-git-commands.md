# Commonly used Git commands

Creation date: 2013-1-1

Tags: *git*

---


## Command list

List all branches, with current branch highlighted.
```
git branch
git branch -vv    # include their HEAD commit, and their default remote repo.
```

List all remotely tracked repos along with their URL.
```
git remote -v
```

List commits.
```
git log
```

Show current local branch's status (untracked files, modified tracked files, indexed files).
```
git status
git status -uno  # untracked files excluded
```

Switch from current working branch to another local branch.
```
git checkout <another-branch>
```

Create a new local working branch based on the current branch, then switch to that new branch.
```
git checkout -b <new-branch-name>
```

Add specified untracked/modified files to Git index (a.k.a. staging area).
```
git add <file-1> [<file-2> [...]]
```

Add all untracked files and modified files to Git index.
```
git add .
```

Commit all changes in Git index as single commit.
```
git commit -m "<brief-commit-message>"
```

Modify last commit's message.
```
git commit --amend
```

Squash last *\<num\>* commits from HEAD as single commit.
Refer [git squash workflow](#squash-workflow) for details.

**WARNING**: Once squashed, you'll lose track on original commits' details (e.g. author, commit 
date). Use it only if makes sense. For example:
- GOOD practice: combine your fragment commits in your working branch for the same task, merging 
  them into single commit, before pushing the task's final commit to team's integration branch.
- BAD practice: combine multiple tasks' commits or multiple people's commits into single commit,
  hence losing track about when/who/why the code change was introduced.
```
git rebase -i HEAD~<num>
```

Push local branch's commits to the default or specified remotely tracked repo.
```
git push
git push <remote-name>
```

Override all commits in the default or specified remotely tracked repo/branch, with current local 
branch's commits.

**WARNING**: Once overridden, lost commits cannot be recovered. Avoid doing this if the remote 
  repo/branch is a shared branch.
```
git push -f
git push -f <remote-name>
```


## Workflows

### Squash workflow
```
# Step 1: Use below command to start squash workflow. replace <num> with number of commits from 
#   HEAD.
git rebase -i HEAD~<num>

# then git will displays those commits from specified range in editor, let say <num> is 3:
pick <commit 3 ID> <commit msg 3>
pick <commit 2 ID> <commit msg 2>
pick <commit 1 ID> <commit msg 1>
... <and some instruction comments for reference. Comments have no effect on workflow so leave them>

# Step 2: Change last 2 commits' command from pick to squash:
pick <commit 3 ID> <commit msg 3>
squash <commit 2 ID> <commit msg 2>
squash <commit 1 ID> <commit msg 1>
# Save the changes and quit editor.

# Step 3: Git will opens another editor for composing commit message for the produced consolidated 
#   commit. Once saved, run git log to verify the outcome. You'll see that last 3 commits have 
#   become 1, having the new commit message.
```
