# Learn Git The Hard Way

## Version Control

When editing, you can Save As. . . a different file, or copy the file somewhere first before saving if you want to savour old versions. You can compress them too to save space. This is a primitive and labour-intensive form of version control. Computer games improved on this long ago, many of them providing multiple automatically timestamped save slots.

Let’s make the problem slightly tougher. Say you have a bunch of files that go together, such as source code for a project, or files for a website. Now if you want to keep an old version you have to archive a whole directory. Keeping many versions around by hand is inconvenient, and quickly becomes expensive.

With some computer games, a saved game really does consist of a directory full of files. These games hide this detail from the player and present a convenient interface to manage different versions of this directory.

Version control systems are no different. They all have nice interfaces to manage a directory of stuff. You can save the state of the directory every so often, and you can load any one of the saved states later on. Unlike most computer games, they’re usually smart about conserving space. Typically, only a few files change from version to version, and not by much. Storing the differences instead of entire new copies saves room.

## Distributed Control

Now imagine a very difficult computer game. So difficult to finish that many experienced gamers all over the world decide to team up and share their saved games to try to beat it. Speedruns are real-life examples: players specializing in different levels of the same game collaborate to produce amazing results.

How would you set up a system so they can get at each other’s saves easily? And upload new ones?

In the old days, every project used centralized version control. A server somewhere held all the saved games. Nobody else did. Every player kept at most a few saved games on their machine. When a player wanted to make progress, they’d download the latest save from the main server, play a while, save and upload back to the server for everyone else to use.

What if a player wanted to get an older saved game for some reason? Maybe the current saved game is in an unwinnable state because somebody forgot to pick up an object back in level three, and they want to find the latest saved game where the game can still be completed. Or maybe they want to compare two older saved games to see how much work a particular player did.

There could be many reasons to want to see an older revision, but the outcome is the same. They have to ask the central server for that old saved game. The more saved games they want, the more they need to communicate.

The new generation of version control systems, of which Git is a member, are known as distributed systems, and can be thought of as a generalization of centralized systems. When players download from the main server they get every saved game, not just the latest one. It’s as if they’re mirroring the central server.

This initial cloning operation can be expensive, especially if there’s a long history, but it pays off in the long run. One immediate benefit is that when an old save is desired for any reason, communication with the central server is unnecessary.

## A Silly Superstition

A popular misconception is that distributed systems are ill-suited for projects requiring an official central repository. Nothing could be further from the truth. Photographing someone does not cause their soul to be stolen. Similarly, cloning the master repository does not diminish its importance.

A good first approximation is that anything a centralized version control system can do, a well-designed distributed system can do better. Network resources are simply costlier than local resources. While we shall later see there are drawbacks to a distributed approach, one is less likely to make erroneous comparisons with this rule of thumb.

A small project may only need a fraction of the features offered by such a system, but using systems that scale poorly for tiny projects is like using Roman numerals for calculations involving small numbers.

Moreover, your project may grow beyond your original expectations. Using Git from the outset is like carrying a Swiss army knife even though you mostly use it to open bottles. On the day you desperately need a screwdriver you’ll be glad you have more than a plain bottle-opener.

## Merge Conflicts

For this topic, our computer game analogy becomes too thinly stretched. Instead, let us again consider editing a document.

Suppose Alice inserts a line at the beginning of a file, and Bob appends one at the end of his copy. They both upload their changes. Most systems will automatically deduce a reasonable course of action: accept and merge their changes, so both Alice’s and Bob’s edits are applied.

Now suppose both Alice and Bob have made distinct edits to the same line. Then it is impossible to proceed without human intervention. The second person to upload is informed of a merge conflict, and must choose one edit over another, or revise the line entirely.

More complex situations can arise. Version control systems handle the simpler cases themselves, and leave the difficult cases for humans. Usually their behaviour is configurable.

---

This file file serves as your book's preface, a great place to describe your book's content and ideas.

Welcome to the world of Git. I hope this book will help to advance your understanding of this powerful content tracking system, and reveal a bit of the simplicity underlying it — however dizzying its array of options may seem from the outside.

Before we dive in, there are a few terms which should be mentioned first, since they’ll appear repeatedly throughout this text:

| Keyword | Description |
| :--- | :--- |
| **repository** | A repository is a collection of commits, each of which is an archive of what the project's working tree looked like at a past date, whether on your machine or someone else's. It also defines _HEAD_, which identifies the branch or commit the current working tree stemmed from. Lastly, it contains a set of _branches_ and _tags_, to identify certain commits by name. |
| **the index** | Unlike other, similar tools you may have used, Git does not commit changes directly from the _working tree_ into the repository. Instead, changes are first registered in something called **the index**. Think of it as a way of “confirming” your changes, one by one, before doing a commit (which records all your approved changes at once). Some find it helpful to call it instead as the “staging area”, instead of the index. |
| **working tree** | A **working tree** is any directory on your filesystem which has a _repository_ associated with it (typically indicated by the presence of a sub-directory within it named .git.). It includes all the files and sub-directories in that directory. |
| **commit** | A **commit** is a snapshot of your working tree at some point in time. The state of HEAD at the time your commit is made becomes that commit’s parent. This is what creates the notion of a “revision history”. |
| **branch** | A **branch** is just a name for a commit (and much more will be said about commits in a moment), also called a reference. It’s the parentage of a commit which defines its history, and thus the typical notion of a “branch of development”. |
| **tag** | A **tag** is also a name for a commit, similar to a _branch_, except that it always names the same commit, and can have its own description text. |
| **master** | The mainline of development in most repositories is done on a branch called **“master”**. Although this is a typical default, it is in no way special. |
| **HEAD** | **HEAD** symbolically refers to that branch, indicating that the branch name should be updated after the next commit operation. • If you checkout a specific commit, HEAD refers to that commit only. This is referred to as a _detached HEAD_, and occurs, for example, if you check out a tag name. |

The usual flow of events is this: After creating a repository, your work is done in the working tree. Once your work reaches a significant point — the completion of a bug, the end of the working day, a moment when everything compiles — you add your changes successively to the index. Once the index contains everything you intend to commit, you record its content in the repository. Here’s a simple diagram that shows a typical project’s life-cycle:

