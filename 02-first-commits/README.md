# 2. First commits

<!-- AUTHOR: optional personal opener, e.g. a time you lost work before you used git. -->

Right now `movies.md` is just a file, and if you mess it up, it's gone. Git fixes that by taking **snapshots** of your project, called **commits**, that you can always go back to. In this chapter you'll turn your `movies` folder into a git repo and start building up a history.

## Turn the folder into a repo

```bash
cd ~/code/movies
git init
```

You should see something like:

```
Initialized empty Git repository in /Users/yourname/code/movies/.git/
```

Now look at everything in the folder, including hidden files:

```bash
ls -a
```

```
.  ..  .git  movies.md
```

That `.git` folder is the repo. Every commit, every branch, the whole history lives in there. Don't edit anything in it by hand (we'll poke around inside it carefully in chapter 4). If you ever delete `.git`, your files stay but the history is gone.

## Ask git what's going on

`git status` is the command you'll run more than any other. Run it whenever you're not sure what state things are in.

```bash
git status
```

You should see something like:

```
On branch main

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	movies.md

nothing added to commit but untracked files present (use "git add" to track)
```

**Untracked** means git can see the file but isn't keeping track of it yet.

## The three places a change lives

This is the part of git that trips most people up, so it's worth getting right now. A change moves through three places:

1. **Working directory**: the actual files on your computer, the ones you edit.
2. **Staging area** (also called the _index_): the changes you've picked to go into the next commit.
3. **Repository**: the commits, saved for good.

`git add` moves changes from 1 to 2. `git commit` moves everything in 2 into 3.

Why the middle step? Because sometimes you've changed five things and only want to commit two of them. Staging lets you pick.

## Stage it

```bash
git add movies.md
git status
```

You should see something like:

```
On branch main

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
	new file:   movies.md
```

## Writing commit messages

Before your first commit, a word about messages, because this is where a lot of people get lazy. A commit message is a note to future you (and your teammates) explaining what changed. `fixed stuff` and `asdf` tell nobody anything.

We're going to use a style called **Conventional Commits**. Every message starts with a **type**, a colon, a space, and a short description:

```
<type>: <description>
```

For now you only need four types:

| Type    | Use it when you...                              | Example                             |
| ------- | ----------------------------------------------- | ----------------------------------- |
| `feat`  | add something new                               | `feat: add coco`                    |
| `fix`   | correct something that was wrong                | `fix: updated pirates score`        |
| `docs`  | change documentation, like a README             | `docs: add readme`                  |
| `chore` | do housekeeping that doesn't change the content | `chore: sort movies alphabetically` |

And a few rules for the description:

- Lowercase, no period at the end.
- Write it like a command: `add coco`, not `added coco` or `adding coco`. A good trick is that it should finish the sentence _"This commit will..."_
- Keep it short, under about 50 characters. **NOTE**: This is not a strict rule, but consider abiding to it. It helps keep the git blame short and sweet. You can always add comments to supplement the changes as well, which is why nice contained commits are generally helpful for a developer.

## Commit it

```bash
git commit -m "feat: add movies list"
```

The `-m` lets you type the message right there. You should see something like:

```
[main (root-commit) 3f2a91c] feat: add movies list
 1 file changed, 5 insertions(+)
 create mode 100644 movies.md
```

That's your first commit! Run `git status` again and git will tell you there's nothing to commit, which means everything you have is saved.

## Look at the history

```bash
git log
```

You should see something like:

```
commit 3f2a91c8e0b4d6a7f1c2e9b8a7d6c5b4a3f2e1d0 (HEAD -> main)
Author: Your Name <you@example.com>
Date:   Mon Sep 28 17:02:11 2026 -0400

    feat: add movies list
```

(If the log takes over your screen, press `q` to get out.)

See that long string of letters and numbers after `commit`? That's the commit's **hash**. Every commit gets one, and it's unique to that exact commit. It's how git (and you) refer to a specific snapshot. It's 40 characters long, but you almost never need the whole thing, the first 7 are enough. Chapter 4 explains where these come from, they're a big deal.

## Change something

Add a movie from VS Code this time. Open the folder with `code .`, add a line to `movies.md`, and save:

```
- Coco (2017) - 9/10
```

Back in the terminal:

```bash
git status
```

You should see something like:

```
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   movies.md

no changes added to commit (use "git add" and/or "git commit -a")
```

Git noticed the file changed. To see _what_ changed:

```bash
git diff
```

You should see something like:

```
diff --git a/movies.md b/movies.md
index 1a2b3c4..5d6e7f8 100644
--- a/movies.md
+++ b/movies.md
@@ -3,3 +3,4 @@
- Pirates of the Caribbean: The Curse of the Black Pearl - 10/10
- The Spongebob Squarepants Movie - 11/10
- Cars - 10/10
+- Coco (2017) - 9/10
```

Lines starting with `+` were added, lines starting with `-` were removed. This is why every movie is on its own line: one change, one line in the diff, easy to read.

Stage and commit it:

```bash
git add movies.md
git commit -m "feat: add coco"
```

## Your turn

Make these two commits on your own. Run `git status` and `git diff` before each one so you can see what's about to happen.

1. Change the rating on one of your movies. Commit it as a `fix`, something like `fix: bump the Spongebob Squarepants to 11/10`.
2. Create a `README.md` in the same folder with one line describing the repo, like `My favorite movies, tracked with git.` Commit it as `docs: add readme`.

Hint: a new file starts out untracked, so `git add` it first. If you want to stage every changed file at once, `git add .` does that (the `.` means _everything in this folder_), just check `git status` first so you know what you're adding.

## Read it back

```bash
git log --oneline
```

You should see something like:

```
b7e4c21 (HEAD -> main) docs: add readme
9d0f3a8 fix: bump the Spongebob Squarepants to 11//10
4c1e7b2 feat: add coco
3f2a91c feat: add movies list
```

Newest is at the top. Look how readable that is! Anyone could look at this and tell you exactly what happened to the repo. That's the whole point of good commit messages.

A couple more ways to look at history:

```bash
git log -n 2          # only the last 2 commits
git show 9d0f3a8      # one commit and exactly what it changed (use one of YOUR hashes)
```

## Recap

| Command                         | Does                                       |
| ------------------------------- | ------------------------------------------ |
| `git init`                      | turns a folder into a repo                 |
| `git status`                    | tells you what's changed and what's staged |
| `git add <file>`, `git add .`   | stages changes                             |
| `git commit -m "type: message"` | saves staged changes as a commit           |
| `git diff`                      | shows unstaged changes                     |
| `git log`, `git log --oneline`  | shows history                              |
| `git show <hash>`               | shows one commit                           |

Next: [branches](../03-branches/), where you try things out without touching `main`.
