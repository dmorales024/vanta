# 6. Ignoring files

Not everything in your folder belongs in git. Some files are junk your computer makes on its own, some are just for you, and some (like passwords) should never, ever end up on GitHub. A `.gitignore` file tells git to pretend those files don't exist.

## Make something to ignore

Say you keep a list of movies you *want* to watch but haven't seen yet, and it's just for you. Make a folder for it:

```bash
cd ~/code/movies
mkdir drafts
echo "- Shrek (2024)" > drafts/watchlist.md
git status
```

You should see something like:

```
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	drafts/

nothing added to commit but untracked files present (use "git add" to track)
```

Git sees it and keeps reminding you about it, and one day you're going to `git add .` and commit it by accident.

## Write a .gitignore

Make a file called `.gitignore` (with the dot) in the top of your repo:

```bash
code .gitignore
```

Put this in it and save:

```
# my personal lists
drafts/

# junk files Macs make in every folder
.DS_Store
```

Now:

```bash
git status
```

You should see something like:

```
On branch main
Your branch is up to date with 'origin/main'.

Untracked files:
  (use "git add <file>..." to include in what will be committed)
	.gitignore
```

`drafts/` disappeared from the list. The folder is still on your computer, git just stopped paying attention to it. The `.gitignore` itself *should* be committed, so everyone who uses the repo ignores the same things:

```bash
git add .gitignore
git commit -m "chore: ignore drafts folder"
git push
```

`chore` fits here because you didn't change any movies, you just did some housekeeping.

## Patterns

Each line in a `.gitignore` is a pattern:

| Pattern | Ignores |
|---|---|
| `notes.txt` | any file named `notes.txt`, in any folder |
| `drafts/` | the folder `drafts` and everything in it |
| `*.tmp` | any file ending in `.tmp` (`*` matches anything) |
| `/todo.md` | only `todo.md` at the top of the repo, not in subfolders |
| `!keep.tmp` | *don't* ignore `keep.tmp`, even though `*.tmp` would |
| `# comment` | nothing, it's a note for humans |

Order matters: later lines win over earlier ones, which is how `!` can undo an earlier pattern.

## Why is this file ignored?

When you can't figure out why git is ignoring something:

```bash
git check-ignore -v drafts/watchlist.md
```

You should see something like:

```
.gitignore:2:drafts/	drafts/watchlist.md
```

That says line 2 of `.gitignore`, the `drafts/` pattern, is the one doing it.

## The catch: already-tracked files

`.gitignore` only affects files git isn't already tracking. If you committed a file and *then* add it to `.gitignore`, git keeps tracking it. To stop, remove it from git without deleting it from your computer:

```bash
git rm --cached <file>
```

Then commit that.

## What should go in a .gitignore?

- **Passwords, keys and secrets.** Ever. Once something is pushed, assume it's public forever, even if you delete it later. Remember chapter 4: it's still in the history.
- **Junk from your OS or editor**, like `.DS_Store` or `Thumbs.db`.
- **Stuff that gets generated**, like build output or downloaded libraries. You'll see `node_modules/` in almost every JavaScript project.
- **Your personal files** that don't belong to the project.

GitHub has a big collection of starter `.gitignore` files for different languages at https://github.com/github/gitignore.

## Your turn

1. Add a pattern so any file ending in `.tmp` is ignored.
2. Make a file called `test.tmp` and check `git status` doesn't show it.
3. Use `git check-ignore -v test.tmp` to see which line is ignoring it.
4. Commit and push the updated `.gitignore` as a `chore:`.

## Recap

| Command | Does |
|---|---|
| `.gitignore` | a file listing patterns for git to ignore |
| `git check-ignore -v <file>` | tells you which pattern ignores a file |
| `git rm --cached <file>` | stops tracking a file but keeps it on disk |

That's the core of git done! You can do real work with just chapters 0 to 6. The rest is **Level 2**, starting with the thing everyone's scared of: [merge conflicts](../07-merge-conflicts/).
