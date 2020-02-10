# Git

Git is a version control tool. Git manages versions of a project with a history of commits. Every commit has a SHA id that makes it uniquely identifiable. Branches are pointers that point to a commit SHA. The current commit is also known as the HEAD. Except for the first commit, every commit has a parent commit that is the previous commit. The `^` character is used to denote a parent commit. So `HEAD^` is the parent of the current commit, and `HEAD^^` is the grandparent of the current commit.

## Basics

`git status` Shows the current status.

`git add {file}` Adds a file to a new commit

`git add .` Adds all modified files to a new commit

`git rm {file}` Removes a file from a new commit. Use the `--cached` flag to remove it from the index.

`git commit -m "{message}"` Creates a message describing the changes made in this commit

`git reset HEAD^` Unstages the files from the most recent commit without altering the files themselves

`pit push {repository} {branch}` Pushes commit to specified repository and branch.

`git fetch` Fetches new commits made to repository

`git pull` Applies changes from origin repo branch to current working branch

`git merge {branch}` Applies changes from the argument specified branch into the current working branch. Git will complain if there are conflicts that need to be resolved.

`git checkout {branch}` Switches to branch. Use `-b` flag for new branch. You can also checkout a specific commit by using the SHA id (you only need the first six characters).

`git stash` Stashes changes made to current branch

`git stash apply` Apply changes that have been stashed to current branch

`git reflog` Views history of changes to the HEAD

## Other Useful Commands

`git diff {file}` Shows changes to file

`git diff {branch1} {branch2}` Compares differences between two branches

`git diff HEAD~{number of commits}` Shows what files have changed since the number of specified commits.

`git diff --check` checks for merge conflicts

`git log` shows a log of commit histories. Use the `--reverse` flag to reverse the order.

`git show` View the most recent changes that were made in the latest commit

`git status -uno` Only shows the current status of staged files. Useful when you have a lot of unstaged files that you are not interested in commiting.

`git checkout -- {file}` Reverts any changes to file to prior commit. Potentially dangerous but useful command.

`git commit --amend -m "{message}"` Amends commit message to be pushed. Useful not only for changing the message, but also for instances where you forgot to add some files to your commit.

`git cherry-pick {SHA id}` Applies changes from specified commit to current commit.

`git rebase -i HEAD~{number of commits}` When you have pushed some commits and need to amend the commit history, `git rebase` can help you fix a larger history. The `-i` flag is for interactive and opens up your default editor to make changes. The bottom of the page has more details on the types of changes you can make.

`git reset --hard HEAD~{number of commits}` Resets branch back to the number of commits specified.

`git clean` Deletes files not tracked by git.

## Writing Git Hooks

In any directory where git is being used there will be a `.git/hooks` file which contains a bunch of sample scripts that take place at different times. This is useful if you want to make sure certain tasks always happen when writing or pushing commits.

Lets say you have a Node.js project and you want to make sure your test suite is always performed for every commit. You would write the following bash script called `pre-commit`:

```bash
#!/bin/bash
set -eu
npm run test
```

After writing this file you need to set it to be executable. `chmod +x pre-commit`. Now your tests will always run. The pre-commit hook will abort a commit if it exits with any code other than 0, so you could also write something more sophisticated where if the tests doesn't pass it exits with an error code.
