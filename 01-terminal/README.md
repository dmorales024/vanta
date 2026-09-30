# 1. The terminal

<!-- AUTHOR: optional personal opener about when the terminal clicked for you. -->
The terminal looks scary because it's a blank box with a blinking cursor, but it's really just a way of telling your computer to do things with words instead of clicks. Git lives in the terminal, so before we touch git at all, you're going to get comfortable moving around in it.

By the end of this chapter you'll have a folder called `movies` with a file in it that you made entirely from the terminal.

## Where am I?

Open your terminal (Git Bash on Windows) and type:

```bash
pwd
```

`pwd` means *print working directory*, it tells you which folder you're currently "standing" in. You should see something like:

```
/Users/yourname
```

On Windows it'll look like `/c/Users/yourname`. That's your **home folder**, and the terminal has a shortcut for it: `~`.

## What's here?

```bash
ls
```

`ls` *lists* what's in the folder you're in. You'll see the same folders you'd see in Finder or File Explorer, like `Desktop`, `Documents`, `Downloads`.

Some files are hidden, usually ones whose names start with a dot. To see everything:

```bash
ls -a
```

The `-a` is called a **flag**. Flags change how a command behaves, and almost every command has a bunch of them. You'll see a lot of flags in this course.

## Moving around

`cd` means *change directory*. Go into the `code` folder you made in setup:

```bash
cd ~/code
pwd
```

You should see something like:

```
/Users/yourname/code
```

A few special names that work with `cd`:

| Command | Takes you to |
|---|---|
| `cd ~` or just `cd` | your home folder |
| `cd ..` | up one folder, to the parent |
| `cd -` | back to wherever you just were |
| `cd code/vanta` | into `code`, then into `vanta`, from wherever you are |

Try going up and back down a few times, and run `pwd` after each one so you can see where you ended up.

## Two things that'll save you a ton of typing

**Tab completion.** Type `cd ~/co` and press `Tab`. The terminal finishes the word for you. If there's more than one match, press `Tab` twice to see them all. Use this constantly, it also stops you from making typos.

**The up arrow.** Press `↑` to bring back the last command you ran. Keep pressing to go further back.

## Making things

Make a folder for your project and go into it:

```bash
cd ~/code
mkdir movies
cd movies
```

Make an empty file:

```bash
touch movies.md
ls
```

You should see:

```
movies.md
```

Now let's put something in it. `echo` just prints whatever you give it:

```bash
echo "hello"
```

The fun part is you can send that output into a file instead of the screen, using `>` or `>>`:

- `>` **replaces** everything in the file
- `>>` **adds** to the end of the file

Give your list a title:

```bash
echo "# My Favorite Movies" > movies.md
```

Then add your first three movies. Every movie goes on its own line and looks exactly like this: a dash, the title, the year in parentheses, a dash, and your rating out of 10.

```bash
echo "- Pirates of the Caribbean: The Curse of the Black Pearl - 10/10" >> movies.md
echo "- The Spongebob Squarepants Movie - 11/10" >> movies.md
echo "- Cars - 10/10" >> movies.md
```

Use your own favorites! The format is what matters, because we'll lean on it later.

Now read the file back with `cat`:

```bash
cat movies.md
```

You should see something like:

```
# My Movies
- Pirates of the Caribbean: The Curse of the Black Pearl - 10/10
- The Spongebob Squarepants Movie - 11/10
- Cars - 10/10
```

If you used `>` where you meant `>>` and wiped the file, no big deal, just redo the lines. (This is exactly the kind of mistake git is going to protect you from starting next chapter.)

## Opening VS Code from here

```bash
code .
```

The `.` means *this folder*. VS Code opens with your `movies` folder, and you can edit `movies.md` there like any normal file. For the rest of the course you'll mix both: the terminal for commands, VS Code for editing.

## Getting out of trouble

- **`Ctrl+C`** stops whatever command is running. If the terminal seems frozen, try this first.
- **`clear`** wipes the screen when it gets messy. Nothing is lost.
- **`q`** gets you out of screens that take over your terminal, like long outputs from `git log` or `git help`.

## Reading help

Every git command has a manual:

```bash
git help init
```

Press `q` to quit it. When you read docs you'll see a pattern like this:

```
git init [<directory>]
```

Anything in `<angle brackets>` is something you fill in, and anything in `[square brackets]` is optional. So `git init` works, and `git init my-folder` works too.

## Try it

Before moving on, without looking back up:

1. Go to your home folder, then back to `~/code/movies`.
2. Add a fourth movie to `movies.md` from the terminal.
3. Print the file to check it.

## Recap

| Command | Does |
|---|---|
| `pwd` | shows where you are |
| `ls`, `ls -a` | lists files, including hidden ones |
| `cd <folder>`, `cd ..`, `cd ~` | moves around |
| `mkdir <name>` | makes a folder |
| `touch <file>` | makes an empty file |
| `echo "text" > file` | replaces a file's contents |
| `echo "text" >> file` | adds a line to the end |
| `cat <file>` | prints a file |
| `code .` | opens this folder in VS Code |

You have a folder and a file, but git doesn't know about any of it yet. That's [chapter 2](../02-first-commits/).
