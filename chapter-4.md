Constructive Conflict

## A Conflict {#a-conflict}

This time, Harry and Isabelle both decide to add a line at the end of the same file. Starting from the scratch directory again:

```
cd harry
echo "Harry's line" 
>
>
 content01
git commit -am "Harry"
git push

```

Notice that I added an`a`in front of the`m`in the`git commit`command. Because I’m not adding any new files, only updating modified files, and I know it’s safe to pick up all my changes in the commit, I can skip the separate`git add`step.

```
cd ../isabelle
echo "Isabelle's line" 
>
>
 content01
git commit -am "Isabelle"
git push

```

As before, the push is rejected. As I mentioned the last time, Isabelle’s change is commited to the repository, so it’s not going anywhere. However, this time, when Isabelle does`git pull`to merge in the changes, Git doesn’t automatically kick off a “merge commit”; instead, it just reports a conflict and waits for us to do something.

Similar to Subversion, Git creates a “conflict” version of the original file, so now`content01`is full of markers like`<<<<<<<< HEAD`. As you might have noticed from the times we used`git log`, Git uses hashes as labels for commits, so the file looks a little messy.

\(A hash is a long alphanumeric string derived from some data, in this case the commit itself. Git uses them because they can be made without asking some central server for the “next sequence number” and are very likely to be unique.\)

## Push The Reset Button {#push-the-reset-button}

Hopefully Isabelle can figure out what the file should look like. If not, it’s easy and safe to reset back to a point before the merge \(try it\):

```
git merge --abort

```

Back in the olden days before electricity, we did this with`git reset --hard HEAD`. It still worked. You will read in Git tutorials that the`--hard`option to HEAD can be unsafe because it modifies the working copy. This is not an issue for us, because we are smart enough to always pull only when all of our changes have been committed \(or stashed\). That’s a good rule to follow, because while`git merge --abort`will try to put your working copy back the way it was, and generally will succeed, if you’ve committed all of your changes you’re guaranteed to be able to get them back.

I haven’t introduced`git reset`yet. Why? Because Harry and Isabelle haven’t made any mistakes. They’re very good at their jobs. But they’ll make some eventually.

## Finishing the merge {#finishing-the-merge}

Finally Isabelle gets a chance to talk to Harry, and they decide that her line should come first. She does

```
git pull

```

again, and gets the conflict back, and this time she edits the file`content01`to resolve the conflict.

After that she can just:

```
git commit -a
git push

```

Note that in this case Isabelle didn’t use the`-m`option to provide a message. This causes Git to pop up an editor with a merge message already built-in, plus a list of the files that were in conflict. That information then becomes part of the log.

At least for me, I use the`-m`switch to`git commit`about 0.1% of the time. It’s just as fast to type a message into the editor, plus in the editor view Git gives you one last chance to verify that the right stuff is being committed. If you don’t like what you see, you can simply exit the editor without saving and the commit will be canceled.

## Git Stash {#git-stash}

Unfortunately, life isn’t always easy. Isabelle might have been in the middle of a change when Harry really needed her to pull his changes and look at them. A commit in Git is small and cheap, so it would be OK for Isabelle to just commit her changes anyway and then come back and fix them later. But Isabelle has another option.

`git stash`creates what is essentially a temporary commit, but it doesn’t apply it to the working copy or upload it to the remote repository on a`push`. Instead, it keeps it in a special separate storage and returns the working copy to the previous “clean” state.

Let’s say things went down like this:

```
cd ../harry
git pull
echo "Third down and 10" 
>
>
 content03
git commit -am "Down and distance"
git push
cd ../isabelle
echo "Third, Third Third" 
>
>
 content03

```

At this point Isabelle wants to pull in Harry’s change, presumably to run it. \(I know, we aren’t writing code, but pretend.\) If she commits her change first, she’ll have to resolve the conflict before she gets a clean working copy with Harry’s changes. So instead:

```
git stash
git pull

```

The changes come in cleanly. After Isabelle is done and is ready to get her changes back, she does:

```
git stash pop

```

At this point she has to deal with the conflict, by editing the file as above and then committing the change.

Yes, the reference to “pop” means that the stash is a stack, and yes, that means it’s possible to have more than one commit on it. However, it’s not a good idea. In fact, while`git stash`is a good thing to know, feature branches typically make it unnecessary under normal circumstances.

## Wrapping Up {#wrapping-up}

If you’ve been following along perfectly \(congratulations\) you just need to edit`content03`in Isabelle’s directory to remove the conflict tags, then do:

```
git add .
git commit -m "Resolve"
git push
cd ../harry
git pull

```

However, if you haven’t been following as closely but want to catch up, it’s entirely possible that you’ve got some conflicts or commits in either Harry’s or Isabelle’s repository that haven’t been resolved yet. Before we go on, you’ll want to make sure both sides have no remaining uncommitted changes and are up-to-date with each other. The file content doesn’t matter as I’ll avoid making any assumptions about how the conflicts were resolved.

Neither the workflow nor the capabilities in this chapter are different in Git from how they are in Subversion. Resolving conflicting changes is painful no matter what. As we work through subsequent chapters I hope to demonstrate some ways that successful Git projects use Git to control the pain of merging. At the very least, they are successful at putting that pain in a box so that they can decide when to experience it. They do this by getting away from making all changes on a single branch; in fact, most successful Git projects are extremely branch-happy.

In the next chapter we’ll make some branches.

