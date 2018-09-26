---
layout: post
title:  "Fixing Git History"
date:   2018-09-26 1:00:00 -0600
categories: Git
---

This post outlines a possible route for revising history in git and on GitHub.

Recently I have run into an issue at work where a single file in the commit history was showing that I had changed all the lines in a file, which was not true. This is most likely due to confusion between Unix line endings and Windows line endings. (This is an issue that keeps arising for me and I plan on doing more research into this and may write a future post on this as well.) There was also a very confusing merge history when I accidentally merged the wrong branch, so the network chart on GitHub's website was extremely confusing to understand.

I decided that the history needed to be amended given that it is the intent to make this code open source and someone could go back to this history one day.

Fixing git history is a fairly complicated process, and can be done multiple ways by either using `git reset` or `git rebase`. The best solution I found to my particular problem, was through using `git reset`.

* [Understanding the Issue](#understanding-the-issue)
* [Step 1: Copy Current State of Repository](#step-1-copy-current-state-of-repository)
* [Step 2: Rest History](#step-2-reset-history)
* [Step 3: Fix Mistakes](#step-3-fix-mistakes)
* [Step 4: Merge/Push Fix to GitHub](#step-4-merge/push-fix-to-github)

## Understanding the Issue

 For simplicity sake this post follows an example based off a [test repository](https://github.com/jurentie/temp-test-history) which you could clone and edit to follow along. This process is very easy to mess up, and could be detrimental to large projects. *I highly encourage testing this process before making any changes on any important content.*

 Below is an example of two separate branches having been merged with master with the `--no-ff` flag to insure no fast-forward. By doing this, GitHub's network chart can reflect commits that happened in a separate branch and then were merged with master by being displayed on a separate line that is either green or blue. The key is to make amends to the history, while still getting this behavior when checking the network chart.

 Below is an example of the second branch merged with master having an issue in one of the commits.

 ![Network Chart With Issues](https://github.com/jurentie/pictures/blob/master/pictures/history-with-issue.png?raw=true)

 &nbsp; &nbsp;

## Step 1: Copy Current State of Repository

In order to fix the history but retain the edits made up to this point, it is best to clone the repository to a safe back up location. This will be helpful in returning the repository to its current state after fixing any issues, which is outlined later in this post. It might be best to create a backup and save it to a USB, CD, or other external device. It is also possible to copy the repository to a separate folder located elsewhere on a local machine, to be extra cautious. The decision of how securely to back up the repository will depend on how important it is to have a backup of the data.

## Step 2: Reset history

Once the current state of the repository is sufficiently backed up, it is safe to being making changes to history. I decided it was best to reset the history in the repository back to the merge before the commit with an issue. There may be more efficient ways of editing the exact commit with problems, but this is the best solution I have managed to find.

Before Reset:
![Merge Before Issue](https://github.com/jurentie/pictures/blob/master/pictures/previous-merge.png?raw=true)

&nbsp; &nbsp;

In the example above, the previous merge before these commits is the commit with the hash `4a3e0c3`. This is where I want to reset the history to. To do this use the following command:

`$ git reset --hard 4a3e0c3`

`--hard` is needed to ensure that the branch is reset to the given commit and changes made past that commit are discarded. If this returns with `More?` replace the shortened commit hash with the full hash code.

After Reset:
![Reset History](https://github.com/jurentie/pictures/blob/master/pictures/reset-to-history.png?raw=true)

&nbsp; &nbsp;

## Step 3: Fix Mistakes

Now that the branch is set back to the history before there were any issues, we will now fix the mistakes. In order to still reflect that these changes were made in a separate branch, first create a new branch.

`$ git checkout -b {newBranchName}`

Once inside this new branch copy the files from the backup repository to the new branch ensuring not to recreate similar mistakes.

To do this in a way that preserves the order of the commits and ensures the same commits are being made to this repository I used the following command in the backed up repository, which should still contain the .git history.

`$ git log --stat`

This will produce a log of all the commits in history and `--stat` will specify exactly which files were edited in each commit.

![Git Log Stat]({{ '/images/git-log-stat.png' | absolute_url }})

**Tip**: `:q` to exit the log.

## Step 4: Merge/Push Fix to GitHub

Now to make these changes and have them be reflected on GitHub...  

1. Switch back to the master branch:  
 `$ git checkout master`

2. Merge the new branch with changes into master(no fast forward):  
`$ git merge --no-ff {newBranchName}`

3. When pushing these changes to GitHub git will try and suggest a 'git pull' as necessary, since the remote branch is now ahead of the local branch. To get around this issue, force push changes to GitHub:  
`$ git push origin master -f`

This should now show the revised history on GitHub, as can be seen below:  

![Fixed History](https://github.com/jurentie/pictures/blob/master/pictures/fixed-history.png?raw=true)

Again, there are probably many different ways to achieve the same results. While I have been working with GitHub for about 3 years now, there is still much for me to learn. If you have a better method of doing this same process, please feel free to [email me](mailto:jurentie@gmail.com), and I will update this post as necessary.

I hope this helps someone fix their mistakes using git. Thanks for reading.

References:
* [Git HowTo: revert a commit already pushed to a remote repository](http://christoph.ruegg.name/blog/git-howto-revert-a-commit-already-pushed-to-a-remote-reposit.html)
