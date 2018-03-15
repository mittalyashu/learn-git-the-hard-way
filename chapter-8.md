Be Not Led Astray

## When It Goes Wrong {#when-it-goes-wrong}

When I started learning Subversion, it didn’t take long before I had to perform that familiar Google search to find out how to “undo” a commit. The general answer for Subversion is that it’s possible, but it’s far easier to just fix whatever is wrong and make a new commit.

That’s very good advice for Git as well. It’s easy to get hung up trying to find a clever Git solution. Especially because Git is so complicated and powerful, there are ways to go back and rewrite history. But to paraphrase Jurassic Park, just because you can doesn’t mean you should.

In particular, anything you’ve pushed should be treated as gospel. Push additional commits to fix issues; don’t try to get clever with stuff you push.

## Some Easy Fixes {#some-easy-fixes}

If you don’t like a change you made to a file and want the most recent committed version back, use`git checkout`:

```
cd harry
echo "I broke this file" 
>
 spear01
git checkout spear01

```

If you made a typo in a commit message and you_haven’t pushed it yet_, you can use`git commit --amend`:

```
echo "As pretty a piece of flesh as any in Messina" 
>
 spear04
git add .
git commit -m "Dogbert"
git commit --amend -m "Dogberry"

```

As I mentioned before, we can leave off the commit message and Git will launch an editor.

## More Substantial Changes {#more-substantial-changes}

If you “staged” a file for commit with`git add`, and you need to make more changes to it before you commit it, just change it and do`git add`again. But if you realize it shouldn’t be committed at all:

```
echo "Tempfile" 
>
 temp01
echo "Good change" 
>
 content05
git add .

```

If we did a commit now, the temp file would get committed. \(This, by the way, is why I avoid using`git commit -m`in real life; better to let Git launch an editor and review what will be committed.\)

```
git reset temp01
git commit -m "Good only"

```

If we had just said`git reset`without parameters both our changes would have been unstaged.

While we’re at it we can add temp files to`.gitignore`so this doesn’t happen again:

```
echo "temp*" 
>
 .gitignore
git add .
git commit -m "ignore"

```

Note that even though we did`git add .`again,`temp01`didn’t get picked up this time. The addition to`.gitignore`takes effect immediately, even before we commit it.

If we really hosed up our working copy, we can get it back easily:

```
rm -fr *
git reset --hard 

```

In this case`--hard`means, “it’s OK to update my working copy”. This is of course a bit dangerous because we lose all uncommitted changes. Also,`temp01`of course didn’t come back.

## Even Bigger Changes {#even-bigger-changes}

So far we haven’t had to fix anything that was actually committed. Again, where possible, a non-Git solution is better; you can always fix the working copy and make a new commit.

That’s not always easy; you may have destroyed the correct version of the file and want to get it back. If you’re using GitHub or some other on-line tool, you can navigate history and copy/paste. But that’s a cop out.

To reach back into history, we need to tell Git what commit we’re interested in. With all the branching, merging, pushing, and pulling, Git doesn’t have a single authoritative place to store commits, so it can’t number them from 1 to n like Subversion. Instead it uses a hash. You can see that hash in`git log`and you can use it as a unique way to refer to a commit; just copy/paste or type the first few characters and Git will know which one you mean.

But usually when we need to reach back into history, we just want to refer to “the commit before the last one” or “two commits ago”. Git uses the term`HEAD`to refer to the most recent commit, and`HEAD@{1}`to the one before that, and so on. So we don’t need to go look up the hash in the history to get to it.

A simple example:

```
echo "broken" 
>
 content01
git commit -am "Breaking it is easy to do"
git checkout HEAD@{1} content01
git commit -am "Fixing isn't much harder"

```

Another:

```
echo "broke" 
>
 content02
git commit -am "Ain't got no home"
echo "broker" 
>
 content02
git commit -am "A no place to roam"
git checkout HEAD@{2} content02
git commit -am "And I sing like a frog"

```

## Wrapping Up {#wrapping-up}

So far we’ve listed ways to fix most things we could do wrong. There are lots of ways in Git to do exactly what I did here, but I like these ways better because they’re the least intrusive way to do it; for the most part we just use Git to get back the content we want, then make a new commit to fix the repository. This is good because there’s less chance of error and because, except for`git commit --amend`, these commits are safe even if we already pushed the bad change.

When we start working with branches, there’s a couple more ways things could go wrong. The techniques provided here will get us out of a lot of those situations, too, but they may not be the most elegant way to get out. Next chapter I’ll talk about better ways to handle those issues.

