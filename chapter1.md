Setup and Committing

## Setting up Git {#setting-up-git}

I’m not going to spend a lot of time talking about installing Git. The audience for this book is familiar with installing software and using a command line, and Git is sufficiently available on various operating systems that I’ll just assume that`git --version`on the command line doesn’t return`command not found`. I’m also going to assume UNIX syntax for other commands.

I’m also not going to talk about GUIs. There are great GUIs out there for Git, and of course support is built into IDEs. But the concepts are the same and the command line is clearer for learning purposes.

One setup item that is important is telling Git who we are. Git keeps permission to modify a repository \(which might be a user name and password or an SSH key\) separate from the information about who made the change. If we don’t configure it, Git will take that “author” information from our current username. That works but it’s more polite to others to make it accurate.

```
git config --global user.name "Your Name"
git config --global user.email "Your Email"

```

This creates a plain text file called`.gitconfig`in your home directory, with a couple lines in it.

## Creating a repository {#creating-a-repository}

In a scratch directory, type:

```
git init repo

```

Assuming the directory`repo`didn’t exist, this makes it and puts a directory in it called`.git`with a bunch of files. This`.git`directory is the repository; we can move that directory anywhere else we want on the file system and that new location will become a Git repository by virtue of having that`.git`directory in it.

The`.git`directory has a file in it called`config`. This file works just like the`.gitconfig`in our home directory, but it applies only to this repository. You can, for example, have a different name and email address for a given repository.

Other than the`.git`directory, the rest of the content is ours to play with. This is our “working copy”, and the concept is pretty much the same as Subversion.

One other important Git configuration file is`.gitignore`. It’s a regular file, and you can have it at any level of the repository. Each line in the file is a pattern. As long as the pattern doesn’t start with`/`it applies recursively.

## Committing {#committing}

For the purpose of this book, all the changes we make will be small and silly. We may as well begin that way:

```
cd repo
echo "Hi there" 
>
 README
git add README
git commit -m "Initial"

```

For me, this is the first thing about Git that I miss when I use a different tool: “What do you mean I need to be connected to a server to commit changes?” This also presents an important “thinking difference” with Git. Your commits should be really, really small. There should be one commit for each independent idea in whatever work you’re doing. Do I obey that rule? No, I don’t; look at my logs and see for yourself. But it’s a good rule. There are lots of other ways to organize things in Git, so we don’t have to try to fit whole features into a single commit.

## Staging {#staging}

In fact, because Git wants your commits to be small, it behaves exactly opposite from Subversion. In Subversion, if you make changes to a repository and type`svn commit`, by default it will pick up all the changed files and assume you want to commit them. Git instead wants you to “stage” a file to show that you want it included in the commit. Nothing is staged for you automatically. Unfortunately, staging a file or files uses`git add`which kind of overloads that term, especially for Subversion users.

Let’s say we do this:

```
echo "Hi yourself" 
>
>
 README
echo "Adding some content" 
>
 content01
git commit -m "README and content"

```

Nothing happens here except that Git tells us that we have changes that are not staged as well as an untracked file. This is irritating, there’s no way to pretend it isn’t. Git itself tells us in the message that we can get around this using`git commit -a`, but that won’t pick up the untracked file!

Since I’m writing a book about Git, I have to find a way to defend this behavior. The key thing to remember is that commits are supposed to be small. The Git folks realize that in the real world, a developer works on several things at once, files get changed, odd files are inadvertently created. Git is biased toward explicit behavior when it comes to choosing what files to commit, so we need to tell it exactly what files belong in this commit. We might do it like this:

```
git add .
git commit -m "README and content"

```

Note the period at the end of the`git add`command, telling Git to add any new and changed files in the current directory. The`git add`command is recursive, so this would also cover subdirectories.

## Removing files {#removing-files}

One more caveat: while this picks up added and modified files, it doesn’t pick up missing files \(files that are being tracked by Git but don’t appear any longer in the working copy\). This is scheduled to change in Git 2.0, but for now there’s a little extra work.

The command`git rm`is designed to stage a file for removal. It will also remove the file from our working copy. If there are lots of deleted files, and we deleted them with regular “rm” rather than the Git version, it’s a pain to do`git rm`with each one, so there’s one more version of the`add`command to help us:

```
touch bad01 bad02 bad03 bad04
git add .
git commit -m "Bad files"
rm bad*
git commit -m "Remove bad files"

```

Again we find that nothing happens because those changes weren’t staged. We can’t do`git rm bad*`because those files are already gone from the working copy. But we can do this:

```
git add -A
git commit -m "Remove bad files"

```

In Git 2.0, this staging of removed files will be the default and`git add .`will work for both cases.

## Wrapping Up {#wrapping-up}

The idea with staging is that Git expects us to identify explicitly what files we want it to commit, by “staging” them. When we stage a file, Git actually makes a copy of it in its own space in`.git`, and that’s what will be committed. The idea here is that as you work, you can add changes to a commit, and then finalize it with a message when you’re ready.

So far we’ve used Git as a single-person version control tool, but of course in the real-world it’s for collaboration by a team. This means we’ve got to start using Git commands that are designed to move commits between repositories. That’s for the next chapter.

