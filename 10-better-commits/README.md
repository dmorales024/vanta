# 10. Better commits

<!-- AUTHOR: optional personal opener, e.g. reading a messy commit history at work. -->
You've been writing conventional commits since chapter 2, and by now your `git log` probably reads pretty cleanly. This last chapter fills in the rest: scopes, commit bodies, breaking changes, and fixing a commit before anyone sees it. Then you'll do one bigger change to your movies list that uses a bit of everything from the course.

## Why bother?

Look at these two logs for the same project:

```
a1b2c3d fixed it
d4e5f6a asdf
b7c8d9e more changes
e0f1a2b update
c3d4e5f WIP
```

```
a1b2c3d fix: correct release year for alien
d4e5f6a feat(horror): add the thing
b7c8d9e docs: explain rating scale in readme
e0f1a2b chore: sort movies alphabetically
c3d4e5f feat: add movies list
```

From the second one you can tell what happened without opening a single commit. From the first you can't even tell which commit to look at. On a team, people read the log *all the time*, to figure out when something broke, to review what changed, and to write release notes, and some tools even read conventional commit messages to do those things automatically.

## More types

You've been using four types. Here are the other common ones:

| Type | For |
|---|---|
| `feat` | something new |
| `fix` | something that was wrong |
| `docs` | documentation only |
| `chore` | housekeeping |
| `refactor` | reorganizing without changing what it does |
| `style` | formatting only, like spacing |
| `test` | adding or fixing tests |

You don't need all of these for a movie list, but you'll see them in real repos.

## Scopes

A **scope** goes in parentheses after the type and says *which part* of the project changed:

```
feat(horror): add hereditary
fix(sci-fi): correct release year for alien
```

In a real codebase the scope is usually a part of the app, like `feat(login):` or `fix(api):`. It's optional, use it when it helps someone skimming the log.

## Commit bodies

`-m` is great for small commits, but sometimes the *why* doesn't fit in 50 characters. Run `git commit` with no `-m` and VS Code opens for you to write a longer message:

```
fix: lower the matrix to 8/10

Rewatched it and the sequels have ruined it for me a little. Still great,
just not a 10 anymore.
```

The rules:

- First line is the normal conventional commit message.
- Then a **blank line**.
- Then as much explanation as you want. Write about *why*, since the diff already shows *what*.

Save and close the tab to make the commit. `git log` shows the whole thing, and `git log --oneline` shows just the first line, which is why that first line has to make sense on its own.

## Breaking changes

A **breaking change** is one that could break something that depends on your project, and it gets a `!` after the type (or the scope):

```
feat!: add a genre to every entry
```

You can also explain it in the body with a `BREAKING CHANGE:` line at the bottom:

```
feat!: add a genre to every entry

BREAKING CHANGE: every line in movies.md now ends with " - <genre>".
Anything that reads the old format needs updating.
```

For a movie list this is a bit dramatic, but imagine a friend wrote a script that reads your `movies.md` to pick a random movie night. If you change the format of every line, their script breaks. The `!` is how you warn people.

## Fixing your last commit

Made a commit and immediately noticed a typo in the message, or forgot a file? If you **haven't pushed yet**:

```bash
# forgot a file? stage it first
git add README.md

# then
git commit --amend
```

VS Code opens with your last message so you can fix it. When you close it, git replaces your last commit with a new one. (New commit, new hash, chapter 4 again!) Because it rewrites history, the same rule applies as rebase: **don't amend something you've already pushed.**

To just fix the message without opening an editor:

```bash
git commit --amend -m "feat: add the iron giant"
```

## Final project: add genres

Time to put it all together. You're going to add a genre to every movie, like this:

```
- The Matrix (1999) - 8/10 - sci-fi
```

1. `git pull` so you're up to date.
2. Make a branch called `add-genres`.
3. Add a genre to the end of every line in `movies.md`.
4. Commit it with a body, as a breaking change, using `git commit` with no `-m`:

   ```
   feat!: add a genre to every entry

   BREAKING CHANGE: every line in movies.md now ends with " - <genre>".
   ```

5. Update `README.md` to explain the new format. Commit it as `docs:`. Make a typo in the message on purpose, then fix it with `--amend`.
6. Switch to `main`, add one new movie in the *old* format, and commit it.
7. Switch back to `add-genres` and rebase it onto `main`. You'll probably get a conflict, fix it (and give that new movie a genre while you're at it).
8. Merge `add-genres` into `main`, delete the branch, and push.
9. Run `git log --oneline --graph` and read your whole history, top to bottom.

Then go look at it on GitHub. That's a real history of a real repo you built from an empty folder!

## Recap

| Pattern | Example |
|---|---|
| type + description | `feat: add coco` |
| with a scope | `fix(horror): correct year for the shining` |
| breaking change | `feat!: add a genre to every entry` |
| body | `git commit` with no `-m`, blank line after the first line |
| fix the last commit | `git commit --amend` (not pushed yet!) |

## That's the course!

Congrats. Now, let's go code a robot up :)
