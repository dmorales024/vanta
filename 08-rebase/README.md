# 8. Rebase

Merging isn't the only way to bring branches together. **Rebase** takes the commits from your branch and replays them on top of another branch, like you'd started your branch from there in the first place. You end up with a straight line of history and no merge commit. I prefer rebasing, only because it keeps the commit history clean and in order, without a merge commit. You can choose what you'd like, but rebasing makes more sense to me.

It's really useful, and it's also the first command in this course that rewrites history, so there's one rule you have to follow. We'll get to it.

## Set it up

Start clean:

```bash
cd ~/code/movies
git switch main
git pull
git switch -c add-scifi
```

Add two sci-fi movies, one commit each:

```bash
git add movies.md
git commit -m "feat: add interstellar"
```

```bash
git add movies.md
git commit -m "feat: add martian"
```

Now go back to `main` and make a change there too. Edit `README.md` this time, so there's no conflict:

```bash
git switch main
```

Add a line to the README, like `Updated whenever I watch something good.`, then:

```bash
git add README.md
git commit -m "docs: mention how often the list updates"
```

Look at it:

```bash
git log --oneline --graph --all
```

You should see something like:

```
* 4b5c6d7 (HEAD -> main) docs: mention how often the list updates
| * 8e9f0a1 (add-scifi) feat: add martian
| * 2b3c4d5 feat: add interstellar
|/
* 6f7a8b9 (origin/main) fix: set the ssm to 8/10
...
```

Same shape as chapter 3. If you merged now, you'd get a merge commit.

## Rebase instead

Go to your branch and rebase it onto `main`:

```bash
git switch add-scifi
git rebase main
```

You should see:

```
Successfully rebased and updated refs/heads/add-scifi.
```

Look again:

```bash
git log --oneline --graph --all
```

```
* c1d2e3f (HEAD -> add-scifi) feat: add martian
* a4b5c6d feat: add interstellar
* 4b5c6d7 (main) docs: mention how often the list updates
* 6f7a8b9 (origin/main) fix: set the matrix to 8/10
...
```

A straight line! Your two sci-fi commits now come _after_ the README commit, like you'd made the branch after it.

Now look closely at the hashes. Before the rebase, your commits were `2b3c4d5` and `8e9f0a1`. Now they're `a4b5c6d` and `c1d2e3f`. **They're different commits.** Remember chapter 4: a commit's hash includes its parent. Rebase gave your commits a new parent, so git had to make brand new commits with the same changes. The old ones are just left behind.

Now merging into `main` is a fast-forward:

```bash
git switch main
git merge add-scifi
git branch -d add-scifi
git push
```

## The rule

> **Never rebase commits that you've already pushed and other people might have.**

Here's why. If you push `add-scifi`, and a teammate pulls it and builds on it, their work sits on top of your _old_ commits. When you rebase, you replace those commits with new ones that have different hashes. Now your history and theirs disagree about what happened, and untangling it is a real mess.

Rebasing your own local branch before you share it is great. Rebasing something that's already out there is not.

## Merge or rebase?

Both are fine, and teams pick one or the other.

|                          | Merge                                                      | Rebase                             |
| ------------------------ | ---------------------------------------------------------- | ---------------------------------- |
| History                  | keeps exactly what happened, including when branches split | a clean straight line              |
| Makes new commits?       | one merge commit                                           | rewrites your branch's commits     |
| Safe on shared branches? | yes                                                        | no, only on your own unpushed work |

A common habit: rebase your own branch onto `main` to catch up, then merge it in.

## Conflicts during a rebase

Rebase replays your commits one at a time, so a conflict can stop it partway through. It looks like the conflicts from chapter 7, and you fix it the same way, but you finish differently:

```bash
# fix the file and remove the markers, then:
git add movies.md
git rebase --continue
```

Or to give up and go back to how it was:

```bash
git rebase --abort
```

One thing that trips people up: during a rebase, "current" and "incoming" in VS Code are flipped compared to a merge, because git is standing on `main` and replaying _your_ commits onto it. Read the actual lines instead of trusting the labels.

## Your turn

1. Make a branch and add a movie to the **end** of the file.
2. On `main`, add a different movie to the **end** of the file and commit it.
3. Switch to your branch and `git rebase main`. You'll get a conflict.
4. Keep both movies, `git add`, and `git rebase --continue`.
5. Fast-forward `main`, delete the branch, and push.

## Recap

| Command                 | Does                                       |
| ----------------------- | ------------------------------------------ |
| `git rebase <branch>`   | replays your commits on top of that branch |
| `git rebase --continue` | keeps going after you fix a conflict       |
| `git rebase --abort`    | cancels the rebase                         |

Those old commits that rebase left behind are actually still in your repo, and in [undoing things](../09-undo/) you'll see how to find them.
