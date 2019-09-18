# Git

Git is a version control tool.

## Basics

`git status` Shows the current status.

`git add {file}` Adds a file to a new commit

`git add .` Adds all modified files to a new commit

`git rm {file}` Removes a file from a new commit

`git commit -m "{message}"` Creates a message describing the changes made in this commit

`pit push {repository} {branch}` Pushes commit to specified repository and branch.

`git fetch` Fetches new commits made to repository

`git pull` Applies changes from origin repo branch to current working branch

`git merge {branch}` Applies changes from the argument specified branch into the current working branch. Git will complain if there are conflicts that need to be resolved.

`git checkout {branch}` Switches to branch. Use `-b` flag for new branch.

`git stash` Stashes changes made to current branch

`git stash apply` Apply changes that have been stashed to current branch

`git reflog` Views history of changes to the HEAD

## Other Useful Commands

`git diff {file}` Shows changes to file

`git diff {branch1} {branch2}` Compares differences between two branches

`git status -uno` Only shows the current status of staged files. Useful when you have a lot of unstaged files that you are not interested in commiting.

`git checkout -- {file}` Reverts any changes to file to prior commit. Potentially dangerous but useful command.

`git commit --amend -m "{message}"` Amends commit message to be pushed. Useful not only for changing the message, but also for instances where you forgot to add some files to your commit.

`git rebase -i HEAD~{number of commits}` When you have pushed some commits and need to amend the commit history, `git rebase` can help you fix a larger history. The `-i` flag is for interactive and opens up your default editor to make changes. The bottom of the page has more details on the types of changes you can make.
