# 7. Merge conflicts

A merge conflict sounds bad, but all it means is that two branches changed the same line, and git isn't going to guess which one you meant. It stops and asks you. Nothing is broken and nothing is lost, you just have to make a decision.

The best way to stop being scared of them is to cause a bunch on purpose, so that's what this chapter is. You'll make one on your laptop, and then one between your laptop and GitHub.

## Conflict 1: two branches, one line

Start from an up-to-date `main`:

```bash
cd ~/code/movies
git switch main
git pull
```

Pick a movie in your list. We'll use The Spongebob Squarepants Movie here, use whatever you've got. Make a branch where you rate it *lower*:

```bash
git switch -c rerate-ssm
```

In `movies.md`, change `- The Spongebob Squarepants Movie (1999) - 10/10` to `11/10`, then:

```bash
git add movies.md
git commit -m "fix: increase the ssm to 11/10"
```

Now go back to `main` and change **the same line** to something different:

```bash
git switch main
```

Change it to `9/10`, then:

```bash
git add movies.md
git commit -m "fix: set the ssm to 9/10"
```

Now merge:

```bash
git merge rerate-ssm
```

You should see:

```
Auto-merging movies.md
CONFLICT (content): Merge conflict in movies.md
Automatic merge failed; fix conflicts and then commit the result.
```

There it is!

## Reading a conflict

```bash
git status
```

```
On branch main
You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
	both modified:   movies.md
```

Open `movies.md` in VS Code. Where the conflict is, git has written both versions into the file, fenced off with markers:

```
<<<<<<< HEAD
- The Spongebob Squarepants Movie (1999) - 11/10
=======
- The SpongeBob Squarepants Movie (1999) - 9/10
>>>>>>> rerate-ssm
```

- Between `<<<<<<< HEAD` and `=======` is **your** version, from the branch you're on (`main`).
- Between `=======` and `>>>>>>> rerate-ssm` is the version from the branch you're merging in.

Everything else in the file merged fine. Only this spot needs you.

## Fixing it

To resolve a conflict, edit the file so it looks the way you want it, and **delete all three marker lines**. You can keep one side, the other side, or write something new. Say you settle on 8:

```
- The ssm (1999) - 8/10
```

VS Code also shows little buttons above the conflict (**Accept Current Change**, **Accept Incoming Change**, **Accept Both Changes**). They do the same thing as editing by hand, use whichever you like. Just make sure no `<<<<<<<`, `=======` or `>>>>>>>` lines are left in the file.

Save, then tell git it's resolved by staging it, and commit:

```bash
git add movies.md
git commit
```

VS Code opens with a message already filled in, `Merge branch 'rerate-ssm'`. That's fine, close the tab to accept it.

```bash
git log --oneline --graph
git branch -d rerate-ssm
git push
```

## Bailing out

If you're in the middle of a conflict and want to go back to how things were before you ran `git merge`:

```bash
git merge --abort
```

No harm done. You can always try again.

## Conflict 2: GitHub vs your laptop

This one happens all the time in real life: you forget to pull, you make a change, and someone (or you, on another computer) already changed the same thing on GitHub.

1. On GitHub, edit `movies.md` and change the rating of one movie. Commit it with a `fix:` message.
2. **Don't pull.** On your laptop, change the *same movie's* rating to something else, and commit that.
3. Try to push:

```bash
git push
```

You should see something like:

```
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'github.com:yourusername/movies.git'
hint: Updates were rejected because the remote contains work that you do not
hint: have locally.
```

GitHub is refusing because it has a commit you don't have. Pull it:

```bash
git pull
```

```
Auto-merging movies.md
CONFLICT (content): Merge conflict in movies.md
Automatic merge failed; fix conflicts and then commit the result.
```

Same conflict as before, just with GitHub's version on one side. This time the markers say `origin/main` or a hash instead of a branch name. Fix it exactly the same way: edit, remove the markers, `git add`, `git commit`, then:

```bash
git push
```

Now it goes through.

## Avoiding conflicts

You can't avoid them completely, and you shouldn't be scared of them, but you can make them rarer:

- **Pull before you start working**, every time.
- **Keep branches short.** The longer a branch lives, the more `main` moves under it.
- **Keep changes small and on their own lines.** This is why every movie is on its own line. If the whole list were one long line, *every* change would conflict with every other change.

## Your turn

1. Make two branches off `main`. On each, add a *different* movie to the **very end** of the file.
2. Merge the first into `main` (no problem), then merge the second.
3. You'll get a conflict even though you changed different movies! Why? (Hint: look at where the markers are.)
4. Resolve it by keeping both movies, commit, delete both branches, and push.

## Recap

| Command | Does |
|---|---|
| `git merge <branch>` | may stop with a conflict |
| `git status` | shows which files are conflicted |
| `git add <file>` | marks a conflict as resolved |
| `git commit` | finishes the merge |
| `git merge --abort` | cancels the merge and goes back |

Next: [rebase](../08-rebase/), the other way to combine branches.
