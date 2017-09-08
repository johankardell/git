# git

## Undo uncommitted changes to a single file

Initial history

    M testfile.cs
    M textfile.txt

In this case, I accidentally modified textfile.txt and want to undo it without undoing the changes made to testfile.cs.

    git checkout -- textfile.txt

Resulting in history:

     M testfile.cs

## Undo commit, but keep changes

Initial history

    * 9b05a59 (HEAD -> master) Added gello.tt
    * ddbf625 Added extra line
    * e9667ce Initial commit

The last commit was accidental and we want to undo the commit, but keep the change in the file. (Soft reset)

    git reset head^1

Resulting in this history

    * ddbf625 (HEAD -> master) Added extra line
    * e9667ce Initial commit

And this git status

    ?? gello.tt

## Undo commit, and remove changes

Initial history

    * dcb55da (HEAD -> master) Added gello.tt
    * ddbf625 Added extra line
    * e9667ce Initial commit

The last commit was accidental and we want to undo the commit and the change in the file. (Hard reset)

    git reset --hard head^1

Resulting in this history

    * ddbf625 (HEAD -> master) Added extra line
    * e9667ce Initial commit

And the file changed in the commit is deleted.

## Adjust last unpushed commit

Typo in last commit:

    * 831133f (HEAD -> master) Addded extra line
    * e9667ce Initial commit

Change last commit

    git commit --amend -m "Added extra line"

Resulting history:

    * ddbf625 (HEAD -> master) Added extra line
    * e9667ce Initial commit

## Rewrite history of feature branch

**Warning - doing this while on a shared feature branch will break the branch for others until they pull the branch again. Sync with your team mates!**

Initial history

    * 3f27b66 (HEAD -> featurebranch) Again... one more line
    * ebd453c Added One More
    * 2654273 Added One
    * ddbf625 (master) Added extra line
    * e9667ce Initial commit

We realize that we want to tag each commit with a Task ID before merge to master.

    git rebase -i ddbf

This will let you select between all commits made on the featurebranch. Change the first word on each line from `pick` to `reword`, and save/close the window. You will now be prompted to rewrite each commit message. In this example I added #task-32 to each commit message. 

    * aed630f (HEAD -> featurebranch) Again... one more line #task-32
    * b4437e2 Added One More #task-32
    * 5db2c4a Added One #task-32
    * ddbf625 (master) Added extra line
    * e9667ce Initial commit

## Git squash

**Warning - doing this while on a shared feature branch will break the branch for others until they pull the branch again. Sync with your team mates!**

On second thought, I don't want this many commits for this feature branch. I want to squash all commits into one before merge to master. (Using the same example as above)

    git rebase -i ddbf

This will open the same window as before, where you get to pick the commits you want to work with. In this example we change `pick` to `squash` (or `s`) for the last two commits, and close the window.

    * 6d5e459 (HEAD -> featurebranch) Added One #task-32
    * ddbf625 (master) Added extra line
    * e9667ce Initial commit

## When rebase fails

Undo a rebase that didn't go as you hoped

    git reset --hard ORIG_HEAD
