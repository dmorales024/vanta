# Git and the Command Line

<!-- AUTHOR: swap this opener for your own reason for making this course. The line below is a placeholder in your style, not a real story. -->
I wanted to put this together because git is one of those tools that everyone uses and nobody really explains, and honestly the best way to learn it is to just break a bunch of stuff in a repo that doesn't matter. So that's what we're doing!

You'll build one small thing the whole way through: a repo called `movies` with a single file, `movies.md`, that lists your favorite movies. It's a tiny project on purpose, because the point is getting comfortable with the terminal, with commits, and with writing commit messages that someone else can actually read.

## How this works

Every chapter is a folder with a `README.md`. Read it top to bottom and type every command yourself, don't copy and paste. Whenever you run something, there's a *You should see something like this* block right after it so you can check your work. Your output won't match exactly (your commit hashes, names and dates will be different) but the shape should.

Every chapter ends with at least one commit in your `movies` repo, so by the end you'll have a history you built from scratch.

If you've used git before, take the [self-check](self-check.md) first. It won't let you skip anything, but it'll show you which chapters to slow down on.

## Chapters

**Core**

| # | Chapter | What you'll do |
|---|---|---|
| 0 | [Setup](00-setup/) | Install git and VS Code, set up your terminal, configure git, get this repo onto your computer |
| 1 | [The terminal](01-terminal/) | Move around folders and make files without touching your mouse |
| 2 | [First commits](02-first-commits/) | Create the `movies` repo, make commits, write good commit messages, read the log |
| 3 | [Branches](03-branches/) | Try ideas on a branch and merge them back |
| 4 | [Under the hood](04-under-the-hood/) | Open up `.git` and see what a commit actually is |
| 5 | [GitHub](05-github/) | Set up SSH keys, push your repo, pull changes back down |
| 6 | [Ignoring files](06-gitignore/) | Keep files out of git on purpose |

**Level 2**

| # | Chapter | What you'll do |
|---|---|---|
| 7 | [Merge conflicts](07-merge-conflicts/) | Cause conflicts on purpose and fix them, locally and with GitHub |
| 8 | [Rebase](08-rebase/) | Replay your work on top of someone else's |
| 9 | [Undoing things](09-undo/) | `restore`, `reset`, and getting "lost" commits back |
| 10 | [Better commits](10-better-commits/) | Scopes, breaking changes, amending, and a clean history |

When you're done, keep the [cheat sheet](cheat-sheet.md) around.

## A few rules

- **Use the terminal for everything git.** VS Code has git buttons, and they're fine later, but for this course we type it.
- **Windows: always use Git Bash**, not PowerShell or Command Prompt. Setup shows you how to make it the default.
- **Commands are shown without a prompt symbol.** Your terminal might show `$` or `%` before your cursor, ignore it and type just the command.
- **Stuck?** Read the error out loud, it usually tells you exactly what's wrong. Then ask!
