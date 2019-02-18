# Git Reflog Micromaterial


## Task - delete a local branch and bring it back to life

[read the reflog documentation here](https://git-scm.com/docs/git-reflog)

The basic idea is to practice *accidentally* deleting one of our
local branches (which I've done a few times), and then finding
it again in the reflog. If you have a version pushed to one of
your remotes, then you wouldn't need to do this. This is in the
case where you didn't push anything yet, and have deleted the
branch that only existed locally.

Scary, right! I used to think so, but don't worry...the reflog
can help us.

One of the best parts of git is how you can recover from almost
anything, so don't be afraid to lose your work, you can almost
always get it back.

We're going to practice doing that now.


## step-by-step

To start, let's check out a new branch and add a simple file:

`git checkout -b new_script`
(or whatever you want to call the branch)

`touch script.js`

You can add some content in it if you want, but that's not
necessary to practice our reflog magic.

Let's commit it so it sticks around.
(the reflog can only show committed work, so if we never
committed, we wouldn't be able to do this, but also we
wouldn't be able to leave the branch without committing,
so if we have a branch, then we have commits on it)

```
git add script.js
git commit -m 'add a new script'
```

Now you will have one new file on your new branch. Let's go back
to our `master` branch and take stock of things.

`git checkout master`

If we want to remind ourselves of what happened in the other branch
we can simply type:

`git log new_script`

and our log should show us the new commit in the other branch.

...now for the fun part!

Imagine that we're cleaning up some of our old and unused branches,
and so we run a simple command to delete this branch locally:

`git branch -D new_script`

this will result in something that looks like

```
Deleted branch new_script (was 51bc649)
```

So now we've deleted it, and can go have some tea or something...

---

...but wait!!!

Oh my goodness...you had some fantastic file updates in there that
you just found out you actually need (note: this happened to
me earlier today)! Disaster, right?!?!

Not so fast...

We can use the reflog to bring this branch and any of its files
back to life!

First, go ahead and access the reflog:

`git reflog`

You should see something like this:
```
9cdc824 (HEAD -> master) HEAD@{0}: checkout: moving from new_script to master
51bc648 HEAD@{1}: commit: add a new script
9cdc824 (HEAD -> master) HEAD@{2}: checkout: moving from master to new_script
```
(the hashes at the beginning will be different, and you might have slightly
different numbers in `HEAD@{#}`, but the important part is that you have
references to where your HEAD has been).

So this is basically a list of the steps you've been going through in your
local copy of the repo. You can see where you cloned down the repo, where
you switched to a new branch, and where you added a new commit.

The reflog doesn't record where we delete branches, but that's okay, since
what we want it for here is to checkout a previous state (commit).

The easy thing to do is look for the commit with the commit message that
had our new file. Here that is:

`51bc648 HEAD@{1}: commit: add a new script`

So to complete the magic, we can simply checkout that particular commit
(it still lives in git even if the branch is no longer listed when we
run a command like `git branch`)

`git checkout HEAD@{1}`

you can also just use the short hash:

`git checkout 51bc648`

...but I personally like the `HEAD@{#}` syntax, since it feels more intuitive to me.

Essentially, the # there is the number of "transitions" or "moves" or "state changes"
ago.

After checking out the commit, our terminal should look something like:
```
Note: checking out 'HEAD@{1}'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by performing another checkout.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -b with the checkout command again. Example:

  git checkout -b <new-branch-name>

HEAD is now at 51bc648 add a new script
```

...don't worry too much about the `detached HEAD` state, we won't be here
long.

So now that we've checked out that commit, we should see something like
this when we run `git status`:

```
HEAD detached at 51bc648
nothing to commit, working tree clean
```

we can confirm that we have our old file back by running `ls`.
(or however you like to inspect the files in the directory)

You can add stuff to the file now if you want, but for our exercise,
we don't need to.

We need to checkout an actual branch now to make these files
stick around, so let's do that with:

`git checkout -b new_script_back_to_life`

Now return to the `master` branch:

`git checkout master`

There are a few ways to get stuff from that other branch into your `master` branch,
but since we only need one file, let's just do it the easy way:

`git checkout new_script_back_to_life new_script.js`  

this syntax means something like
<pre>
(git checkout     FROM_BRANCH        FILE_NAME)
</pre>

And now, if you run `git status`, you should see the new file in the output:

```
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        new file:   new_script.js

```

Awesome! We brought it back! Now just add it, commit it, and you can continue
doing whatever other great things you were doing in this project.

That's it! We're done!
