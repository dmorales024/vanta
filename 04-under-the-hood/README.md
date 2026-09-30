# 4. Under the hood

I've been using git since I was 16 years old. I learned the commands from an online tutorial but never really explored what else I could do. I just knew my `add`, `commit`, `pull`, `push`, and `status`. Everything else felt too complex, and that I'd never need that information for work. 6 years later, I took a class that went through some of the fundamentals of git and I found out how much love and passion went into the CLI tool itself. There's a plethora of features that are built for super specific use cases or just general niceties that are now overshadowed by AI taking over your computer. You can use git for years without knowing how it works inside, and a lot of people do. But, git gets way less confusing once you've seen that the whole thing is built from a few simple pieces, and the hash is the piece that ties it all together. So in this chapter we open up `.git` and look.

Nothing in this chapter changes your repo until the very end, it's all looking. **Don't edit or delete anything inside `.git` by hand.**

## Porcelain and plumbing

Git's commands come in two kinds. **Porcelain** commands are the nice ones you've been using, like `add`, `commit`, `log` and `merge`. **Plumbing** commands are the low-level ones those are built on. You'd rarely use plumbing day to day, but it's the best way to see what's really in a repo.

## Look inside .git

```bash
cd ~/code/movies
ls .git
```

You should see something like:

```
COMMIT_EDITMSG  HEAD  config  description  hooks  index  info  logs  objects  refs
```

Three of these matter for now: `HEAD`, `refs` and `objects`.

## HEAD and branches are just files

```bash
cat .git/HEAD
```

```
ref: refs/heads/main
```

`HEAD` is a text file that says "we're on `main`". Now follow it:

```bash
cat .git/refs/heads/main
```

You should see something like:

```
5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c1d2e3f4a
```

That's it. A branch is a file with a commit hash in it. When you commit, git writes the new hash into that file. When you `git switch -c`, git makes a new file. That's why branches are so cheap in git, they cost you one tiny text file.

You can ask git for the same thing without reading files:

```bash
git rev-parse HEAD
```

## Everything is an object

Every commit, every version of every file, everything git saves goes into `.git/objects`. There are three kinds you care about:

| Object     | What it is                                                                             |
| ---------- | -------------------------------------------------------------------------------------- |
| **blob**   | the contents of one file                                                               |
| **tree**   | a folder: a list of names pointing to blobs (and other trees)                          |
| **commit** | a pointer to one tree (the snapshot), plus the parent commit, author, date and message |

Every object is named by its hash, and the hash is calculated from the object's contents. The easiest way to see how they fit together is to take one commit apart, starting from the top.

## Take apart a commit

Get the hash of your latest commit with `git log --oneline -n 1`, then ask git what kind of object it is:

```bash
git cat-file -t 5f6a7b8
```

```
commit
```

`-t` asks for the _type_. Now print it with `-p` (_pretty print_):

```bash
git cat-file -p 5f6a7b8
```

You should see something like:

```
tree 8e9f0a1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f
parent 7a8b9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b
parent 2d3e4f5a6b7c8d9e0f1a2b3c4d5e6f7a8b9c0d1e
author Your Name <you@example.com> 1759093331 -0400
committer Your Name <you@example.com> 1759093331 -0400

Merge branch 'add-comedy'
```

That's the entire commit. If your latest is a merge commit, it has two `parent` lines, just like the graph showed you. A normal commit has one, and your very first commit has none.

Now follow the `tree` line (use your own hash):

```bash
git cat-file -p 8e9f0a1
```

```
100644 blob 1b2c3d4e5f6a7b8c9d0e1f2a3b4c5d6e7f8a9b0c	README.md
100644 blob 9c0d1e2f3a4b5c6d7e8f9a0b1c2d3e4f5a6b7c8d	movies.md
```

The tree is your folder: two files, each pointing to a blob. Follow the `movies.md` blob:

```bash
git cat-file -p 9c0d1e2
```

And there's your movie list, exactly as it was in that commit.

So a commit is a chain: **commit → tree → blobs**. And each commit points back at its parent, which points back at _its_ parent, all the way to your first commit. That chain is your history.

## Where the hashes come from

A hash is what you get when you run some data through a hash function (git uses one called SHA-1). The same input always gives the same hash, and even a tiny change to the input gives a completely different one.

Try it. `git hash-object` tells you what hash git _would_ give some content:

```bash
echo "- Coco (2017) - 9/10" | git hash-object --stdin
```

You should see:

```
436a2baecf199ea8d55b90ae685b4478d89b7ebc
```

That's exactly what you should see, character for character, as long as you typed the line exactly. Run it again. Same hash. Ask a friend to run it on their computer, they'll get the same hash too! Change `9/10` to `8/10` and you get something totally different.

This is the big idea, and it's why hashes are one of the most important things in git:

- **Same content means same hash.** If `README.md` didn't change between two commits, both trees point at the _same_ blob. Git doesn't store it twice. That's how git saves full snapshots every commit without your repo getting huge.
- **Different content means a different hash.** A commit's hash comes from its tree, its parent, its author, its date and its message. Change _anything_, even one letter of an old commit message, and that commit gets a new hash. Since the next commit includes that hash as its parent, _its_ hash changes too, and so on all the way to the newest commit.

That second point means nobody can quietly change history without every hash after it changing. It's also why rewriting commits that other people already have causes trouble, which comes back in chapters 8 and 9.

## See the snapshots are shared

Compare the trees of your last two commits:

```bash
git cat-file -p 'HEAD^{tree}'
git cat-file -p 'HEAD~1^{tree}'
```

(The quotes stop your terminal from trying to do anything clever with the `^` and `{}`.)

`HEAD~1` means _one commit before HEAD_, and `^{tree}` means _that commit's tree_. If a file didn't change between those commits, it has the same blob hash in both lists. Find one!

## Config, one level deeper

You set up git in chapter 0 with `--global`. Those settings live in a plain text file:

```bash
cat ~/.gitconfig
```

There's also a config file _inside each repo_:

```bash
cat .git/config
```

Settings in the repo's config win over your global ones, but only for that repo. For example, if you wanted this one repo to use a different email (DO NOT ACTUALLY RUN THIS):

```bash
git config user.email "other@example.com"
```

(No `--global`, so it only goes in `.git/config`.) Don't actually do that one, just know it exists. To see every setting and which file it came from:

```bash
git config --list --show-origin
```

Personally, I've never found the opportunity to use this.

## Your turn: predict a hash

This one ends with a commit.

1. Add one more movie to `movies.md` in VS Code and save. Don't commit yet.
2. Ask git what the new file's hash will be:

   ```bash
   git hash-object movies.md
   ```

   Write that hash down.

3. Commit it with a `feat:` message.
4. Now dig into the new commit with `git cat-file -p` until you find the blob for `movies.md`. Does it match the hash you wrote down?

It should. Git told you the file's hash before the commit even existed, because the hash only depends on what's in the file.

## Recap

| Command                           | Does                                   |
| --------------------------------- | -------------------------------------- |
| `git cat-file -t <hash>`          | shows an object's type                 |
| `git cat-file -p <hash>`          | prints an object                       |
| `git hash-object <file>`          | shows what hash some content would get |
| `git rev-parse HEAD`              | prints the full hash HEAD points at    |
| `HEAD~1`, `HEAD~2`                | one or two commits back                |
| `git config --list --show-origin` | every setting and where it came from   |

Next up, [GitHub](../05-github/), where your repo finally leaves your computer.
