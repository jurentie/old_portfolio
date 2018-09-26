---
layout: post
title:  "Fixing Git History"
date:   2018-09-26 1:00:00 -0600
categories: Git
---

In adding to StateDMI and pushing commits to cdss-app-statedmi-main an issue as arisen where all of the lines in StateDMI_Processor.java were committed as having been changed. This is most likely an issue stemming from Windows line endings versus Unix line endings. Regardless of what caused the issue it would be nice to go back in history and revise the commit to reflect the lines actually edited. This documentation walks through how to do that.

## Understanding the Issue

 For simplicity sake this documentation follows an example based off a test repository. Below is an example of two separate branches having been merged with master with the `--no-ff` flag to insure no fast-forward. By doing this, GitHub's network chart can reflect commits that happened in a separate branch and then were merged with master by being displayed on a separate line that is either green or blue. The key is to make amends to the history, while still getting this behavior when checking the network chart.

 Below is an example of the second branch merged with master having an issue in one of the commits.

 ![Network Chart With Issues](https://github.com/jurentie/pictures/blob/master/pictures/history-with-issue.png?raw=true)

## Step 1: Copy Current State of repository

In order to fix the history but retain the edits made up to this point, it is best to clone the repository to a safe back up location. It might be best to create a backup and save it to a USB, CD, or other external device. It is also possible to simply copy the repository to a separate folder located elsewhere on a local machine. The decision of how securely to back up the repository will depend on how important it is to have a backup of the data.

## Step 2: Reset history

Once the current state of the repository is sufficiently backed up, it is safe to reset the history in the repository back to the merge before the commit with an issue. There may be more efficient ways of editing the exact commit with problems, but this is the best solution I have managed to find.

![Merge Before Issue](https://github.com/jurentie/pictures/blob/master/pictures/previous-merge.png?raw=true)

In the example above, the previous merge before these commits is the commit with the hash `4a3e0c3`. This is where to reset history to. To do this use the following command:

`$ git reset --hard 4a3e0c3`

`--hard` is needed to ensure that the branch is reset to the given commit and changes made past that commit are discarded. If this returns with `More?` replace the shortened commit hash with the full hash code.

## Step 3: Fix Mistakes

Now that the branch is set back to history before there were any issues, it is possible to fix the mistakes. In order to still reflect that these changes were made in a separate branch, first create a new branch.

`$ git checkout -b {newBranchName}`

Once inside this new branch copy the files from the backup repository, ensure that there are no longer similar issues, and commit them to this new branch.

## Step 4: Merge/Push Fix to GitHub

Now to make these changes reflect on GitHub...  

Switch to master branch:  
 `$ git checkout master`

Merge branch with changes (no fast forward):  
`$ git merge --no-ff {newBranchName}`

When pushing these changes to GitHub git will try and suggest a 'git pull' as necessary, since the remote branch is now ahead of the local branch. To get around this issue, force push changes to GitHub:  
`$ git push origin master -f`

This should now show the revised history on GitHub, as can be seen below:  

![Fixed History](https://github.com/jurentie/pictures/blob/master/pictures/fixed-history.png?raw=true)
