# 9. Undoing things

Everybody messes up in git. You commit the wrong thing, you write a bad message, you delete something you needed. The good news is that git almost never actually loses anything once it's been committed, so most mistakes are fixable if you know which command to reach for.

This chapter goes from the gentle undos to the dangerous one, and then shows you the safety net.

## Undo changes you haven't committed

You edited `movies.md`, hated it, and want it back the way it was at the last commit:

```bash
git restore movies.md
```

Try it: mess up a line in `movies.md`, save, check `git diff`, run `git restore movies.md`, and check `git diff` again. It's empty.

**Careful:** this one really is gone. Changes you never committed aren't in git, so git can't bring them back. This is why it is always important to commit often once you get something working!!

## Unstage something

You ran `git add` on a file you didn't mean to:

```bash
git restore --staged movies.md
```

This takes it out of the staging area but leaves your changes in the file. Nothing lost.

> `git status` tells you both of these commands, in its hints. It's worth actually reading them! RTFM (Read the Friendly Manual) as they say :)

## Undo a commit, keep the changes

You just committed with a typo in the message, or you forgot to include a file. Undo the commit but keep everything:

```bash
git reset --soft HEAD~1
```

`HEAD~1` means *the commit before this one* (remember chapter 4). `reset` moves your branch back to it. `--soft` means your changes stay staged, exactly how they were right before you committed, so you can fix things and commit again.

Try it:

```bash
# add a movie, then commit with a bad message
git add movies.md
git commit -m "stuff"

# undo it
git reset --soft HEAD~1
git status          # your change is still staged
git commit -m "feat: add the incredibles"
```

## Throw away commits completely

```bash
git reset --hard HEAD~1
```

`--hard` moves the branch back **and** makes your files match that commit. Your last commit and all its changes disappear from your files. Any uncommitted changes you had disappear too, and those are gone for real.

Use `--hard` when you really do want to throw something away, and run `git status` first so you know what you're throwing away.

## The safety net: reflog

When you reset or rebase, the old commits aren't deleted right away, your branch just stops pointing at them. Git keeps a private diary of everywhere `HEAD` has been, called the **reflog**:

```bash
git reflog
```

You should see something like:

```
9a8b7c6 (HEAD -> main) HEAD@{0}: reset: moving to HEAD~1
f1e2d3c HEAD@{1}: commit: feat: add wall-e
9a8b7c6 (HEAD -> main) HEAD@{2}: commit: feat: add the incredibles
...
```

That `f1e2d3c` is the commit you just threw away with `--hard`. It's still there! Get it back:

```bash
git reset --hard f1e2d3c
```

Scroll further down the reflog and you'll even find the old sci-fi commits from before your rebase in chapter 8.

The reflog is only on your computer (it doesn't get pushed), and git cleans out old entries after a few months, but for everyday "oh no" moments it's got you covered.

## Undoing something you already pushed

Remember the rule from chapter 8: don't rewrite history other people already have. `reset` rewrites history, so it's the wrong tool for commits that are already on GitHub.

Instead, use `revert`. It makes a **new** commit that does the exact opposite of an old one:

```bash
git revert <hash>
```

VS Code opens with a message like `Revert "feat: add wall-e"`. Accept it, then push. History only moves forward, so nobody's copy breaks.

## Which one do I use?

| I want to... | Use |
|---|---|
| throw away changes I haven't committed | `git restore <file>` |
| unstage a file | `git restore --staged <file>` |
| redo my last commit (not pushed yet) | `git reset --soft HEAD~1` |
| throw away my last commit (not pushed yet) | `git reset --hard HEAD~1` |
| get back something I reset or rebased away | `git reflog`, then `git reset --hard <hash>` |
| undo a commit that's already pushed | `git revert <hash>` |

## Your turn

1. Add a movie and commit it. Push it.
2. Decide you don't like that movie after all. Since it's pushed, undo it with `git revert`, and push the revert.
3. Add another movie and commit it, **don't push**. Throw it away with `git reset --hard HEAD~1`.
4. Get it back using `git reflog`. Push it.
5. Run `git log --oneline` and read through what happened. Every step is in there, except the reset, and that's exactly how it should be.

## Recap

| Command | Does |
|---|---|
| `git restore <file>` | discards uncommitted changes to a file |
| `git restore --staged <file>` | unstages a file |
| `git reset --soft <commit>` | moves the branch back, keeps changes staged |
| `git reset --hard <commit>` | moves the branch back and throws changes away |
| `git reflog` | shows everywhere HEAD has been |
| `git revert <hash>` | makes a new commit that undoes an old one |

Last chapter: [better commits](../10-better-commits/).
