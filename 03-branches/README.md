# 3. Branches

<!-- AUTHOR: optional personal opener, e.g. how your team uses branches at work or in FTC. -->
So far every commit has gone in a straight line on `main`. A **branch** lets you go off in a different direction, try something out, and then either bring it back into `main` or throw it away, without ever messing up the version that works. In the software world, pretty much nobody commits straight to `main`, everybody works on a branch.

In this chapter you'll add a whole genre of movies on a branch and merge it back in.

## What's a branch, really?

A branch is just a **name that points at a commit**. That's it. `main` is a name pointing at your latest commit. When you make a new commit on `main`, the name moves forward to it.

`HEAD` is git's way of saying *where you are right now*. Usually it points at a branch, which points at a commit.

See your branches:

```bash
cd ~/code/movies
git branch
```

You should see:

```
* main
```

The `*` shows which branch you're on.

## Make a branch and switch to it

You're going to add some horror movies, but on their own branch first:

```bash
git switch -c add-horror
```

`switch` moves you to a branch, and `-c` *creates* it first. You should see:

```
Switched to a new branch 'add-horror'
```

> You'll also see `git checkout -b add-horror` online, which does the same thing. `checkout` is the older command that does about five different jobs, and `switch` was made to do just this one. Either are acceptable, but I myself am using `switch`.

Now add two horror movies to `movies.md` (your own picks), and commit each one separately:

```bash
git add movies.md
git commit -m "feat: add Kujo"
```

```bash
git add movies.md
git commit -m "feat: add The Shining"
```

## Look at both branches

```bash
git log --oneline --graph --all
```

You should see something like:

```
* e1f2a3b (HEAD -> add-horror) feat: add The Shining
* c4d5e6f feat: add Kujo
* b7e4c21 (main) docs: add readme
* 9d0f3a8 fix: bump the  to 10/10
* 4c1e7b2 feat: add coco
* 3f2a91c feat: add movies list
```

`main` is still back where you left it, and `add-horror` is two commits ahead. You're going to use `git log --oneline --graph --all` a lot, it's the best way to see what's actually going on.

Now switch back to `main` and look at the file:

```bash
git switch main
cat movies.md
```

The horror movies are gone! Well, not gone, they're safe on the other branch. Switching branches changes the files in your folder to match that branch. Switch back to `add-horror`, `cat` it again, and they're back.

## Merge it in

When you're happy with a branch, you **merge** it into `main`. You always merge *into* the branch you're on, so go to `main` first:

```bash
git switch main
git merge add-horror
```

You should see something like:

```
Updating b7e4c21..e1f2a3b
Fast-forward
 movies.md | 2 ++
 1 file changed, 2 insertions(+)
```

**Fast-forward** means `main` hadn't changed since you made the branch, so git just slid the `main` name forward to where `add-horror` was. No new commit needed.

The branch did its job, so delete it:

```bash
git branch -d add-horror
```

(Deleting a branch doesn't delete the commits, they're part of `main` now. It just removes the name.)

## When both branches move

Fast-forwards are the easy case. Usually, while you're working on a branch, `main` changes too. So make that happen on purpose.

Make a new branch for animated movies and add one:

```bash
git switch -c add-animated
```

Add an animated movie to the **bottom** of `movies.md` and commit it:

```bash
git add movies.md
git commit -m "feat: add spider-verse"
```

Now go back to `main` and make a different change, update the `README.md` this time so it doesn't touch the same lines:

```bash
git switch main
```

Change `README.md` to say something like `My favorite movies, tracked with git. Ratings are out of 10.`, then:

```bash
git add README.md
git commit -m "docs: explain rating scale"
```

Look at the graph:

```bash
git log --oneline --graph --all
```

You should see something like:

```
* 7a8b9c0 (HEAD -> main) docs: explain rating scale
| * 2d3e4f5 (add-animated) feat: add spider-verse
|/
* e1f2a3b feat: add the shining
* c4d5e6f feat: add get out
...
```

The history split into two! Now merge:

```bash
git merge add-animated
```

This time git can't just slide `main` forward, because both branches have new commits. So it makes a brand new **merge commit** that ties them together, and it opens VS Code so you can edit the message. The default message, `Merge branch 'add-animated'`, is fine. Close the tab to accept it.

You should see something like:

```
Merge made by the 'ort' strategy.
 movies.md | 1 +
 1 file changed, 1 insertion(+)
```

And the graph:

```bash
git log --oneline --graph --all
```

```
*   5f6a7b8 (HEAD -> main) Merge branch 'add-animated'
|\
| * 2d3e4f5 (add-animated) feat: add spider-verse
* | 7a8b9c0 docs: explain rating scale
|/
* e1f2a3b feat: add the shining
...
```

The merge commit has **two parents**, one from each branch. Clean up:

```bash
git branch -d add-animated
```

> What if both branches change the *same line*? Git can't decide which one wins, so it stops and asks you. That's a **merge conflict**, and it's the whole of chapter 7. For now, keep your branches touching different lines.

## Renaming a branch

Named a branch wrong? From that branch:

```bash
git branch -m new-name
```

This is also how old repos rename `master` to `main`.

## Your turn

1. Make a branch called `add-comedy`.
2. Add two comedies, one commit each, with good messages.
3. Switch to `main` and make a `fix` commit that changes the rating on a movie **near the top** of the file.
4. Merge `add-comedy` into `main`. You'll get a merge commit.
5. Delete the branch and check `git log --oneline --graph --all`.

## Recap

| Command | Does |
|---|---|
| `git branch` | lists branches |
| `git switch -c <name>` | creates a branch and switches to it |
| `git switch <name>` | switches to a branch |
| `git merge <name>` | merges that branch into the one you're on |
| `git branch -d <name>` | deletes a merged branch |
| `git branch -m <name>` | renames the current branch |
| `git log --oneline --graph --all` | shows every branch as a graph |

Now you've got a bunch of commits and a couple of hashes you've been ignoring. Time to find out what they are, in [under the hood](../04-under-the-hood/).
