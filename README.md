# Git Reflog Micromaterial

## Task - delete a local branch and bring it back to life

[read the documentation here](https://git-scm.com/docs/git-reflog)

The basic idea is to practice *accidentally* deleting one or our
local branches (which I've done a few times), and then finding
it again in the reflog.

One of the best parts of git is how you can recover from almost
anything, so don't be afraid to lose your work, you can almost
always get it back.

We're going to practice doing that now.

## step-by-step

so to start, let's check out a new branch and add a simple file:

`git checkout -b new_script`
(or whatever you want to call the branch)

`touch script.js`

then we can commit it so it sticks around

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
deleted branch new_script (was kd903ijlkng)
```

so now we've deleted it.

but wait!!!

Oh my goodness...you had some fantastic file updates in there that
you just found out you actually need! Disaster!

not so fast...

We can use the reflog to bring this branch and any of its files
back to life!

First, go ahead and access the reflog:
`git reflog`

You should see something like this:


...save and exit, and that's it! Unless there were any conflicts, we're done!

## afterwards

run `npm run check` to see if you successfully expunged that dirt
