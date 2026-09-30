# 0. Setup

<!-- AUTHOR: optional personal opener, e.g. the first time you set up git and what went wrong. -->
This is the only chapter where Mac, Windows and Linux do different things. Once you're through it, every command in the course is exactly the same no matter what computer you're on, so take your time here and it'll save you a headache later.

By the end of this chapter you'll have:

- git installed
- VS Code installed, which is what you'll use to edit files
- a terminal that you know how to open
- git configured with your name and email
- a GitHub account
- this course on your computer

Jump to your operating system, then everyone meets back up at [Configure git](#configure-git).

- [Windows](#windows)
- [Mac](#mac)
- [Linux](#linux)

---

## Windows

You're going to install three things, **in this order**, because the git installer looks for the other two and sets itself up around them.

### 1. Windows Terminal

On Windows 11 you probably already have it. Press the Windows key, type `Terminal`, and see if it shows up. If it doesn't, install **Windows Terminal** from the Microsoft Store.

### 2. VS Code

Download it from https://code.visualstudio.com and run the installer with the default options.

### 3. Git for Windows

Download the installer from https://git-scm.com/install/windows. Grab the **x64** version unless you know you have an ARM laptop (you can check in Settings > System > About > System type).

Run it. There are a lot of screens, and for almost all of them you just click **Next**. Only change these three:

| Screen | Change it to |
|---|---|
| **Select Components** | Check **Add a Git Bash Profile to Windows Terminal** (it's unchecked by default) |
| **Choosing the default editor used by Git** | **Use Visual Studio Code as Git's default editor** |
| **Adjusting the name of the initial branch in new repositories** | **Override the default branch name for new repositories**, and leave it as `main` |

Everything else, leave alone and click Next, then Install.

### 4. Make Git Bash your default terminal

1. Close every Windows Terminal window, then open Windows Terminal again.
2. Press `Ctrl+,` to open Settings.
3. On the **Startup** page, set **Default profile** to **Git Bash**, then click **Save**.
4. Close Terminal and open it one more time.

It should open straight into Git Bash. Check with:

```bash
echo $SHELL
```

You should see something like:

```
/usr/bin/bash
```

If it says anything about PowerShell, or **Git Bash** wasn't in the list at all, the checkbox from step 3 got skipped. Run the git installer again and check it this time.

### Windows things to know

- Your home folder `~` is `C:\Users\<your name>`, but Git Bash writes it as `/c/Users/<your name>`. Same place, different spelling.
- **Copy and paste** in Windows Terminal is `Ctrl+Shift+C` and `Ctrl+Shift+V`. Plain `Ctrl+C` *stops* whatever is running, it doesn't copy.
- You might see a warning like `LF will be replaced by CRLF`. That's just Windows and Mac disagreeing about invisible line endings, it's harmless.

Now go to [Configure git](#configure-git).

---

## Mac

### 1. Open Terminal

Press `Cmd+Space`, type `Terminal`, and hit Return. Keep it in your dock, you'll be using it a lot.

### 2. Install git

Type this and press Return:

```bash
git --version
```

If git is already installed you'll see a version number and you're done with this step. If not, a pop-up will offer to install the **Command Line Tools**. Click **Install**, agree to the license, and wait a few minutes while it downloads. Then run `git --version` again.

You should see something like:

```
git version 2.39.5 (Apple Git-154)
```

The exact number doesn't matter.

### 3. VS Code

Download it from https://code.visualstudio.com, open the download, and drag **Visual Studio Code** into your Applications folder.

Then there's one extra step so your terminal can open VS Code. Open VS Code, press `Cmd+Shift+P`, type `shell command`, and choose **Shell Command: Install 'code' command in PATH**.

Close Terminal and open it again, then check it worked:

```bash
code --version
```

You should see a version number and not `command not found`.

<!-- AUTHOR: verify the 'Install code command in PATH' step on a current Mac before class. -->

### Mac things to know

- Your prompt ends in `%` instead of `$`. Doesn't matter.
- Your home folder `~` is `/Users/<your name>`.

Now go to [Configure git](#configure-git).

---

## Linux

Open a terminal (on Ubuntu it's `Ctrl+Alt+T`) and install git.

Ubuntu, Debian, Mint:

```bash
sudo apt update
sudo apt install git
```

Fedora:

```bash
sudo dnf install git
```

Then install VS Code from https://code.visualstudio.com (grab the `.deb` or `.rpm`), and check both:

```bash
git --version
code --version
```

Your home folder `~` is `/home/<your name>`.

---

## Configure git

Everyone does this part, on every computer you use git on. These settings live in a file called `~/.gitconfig` and git reads them every time it runs.

Type each line, and put in your real name and the email you use (or will use) for GitHub:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --global core.editor "code --wait"
git config --global pull.rebase false
```

What each one does:

- `user.name` and `user.email` get stamped on every commit you make, so people know who made it.
- `init.defaultBranch main` names your first branch `main`. Older git calls it `master`, and you'll still see that name around.
- `core.editor "code --wait"` means that when git needs you to type something longer, it opens VS Code instead of Vim. (Vim is great, but getting stuck in it on day one is a rite of passage we're skipping.)
- `pull.rebase false` tells git how to handle `git pull`. We'll get to what that means in chapter 5.

Now check them:

```bash
git config --global --list
```

You should see something like:

```
user.name=Your Name
user.email=you@example.com
init.defaultbranch=main
core.editor=code --wait
pull.rebase=false
```

If you made a typo, just run that line again with the right value and it'll overwrite it.

---

## GitHub account

If you already have a GitHub account, skip this.

Go to https://github.com and sign up. Use the same email you put in `user.email` above, that's how GitHub knows your commits are yours. Pick a username you won't be embarrassed by in five years ;)

You don't need to set anything else up on GitHub yet, chapter 5 handles that.

---

## Get this course onto your computer

Last step! Make a folder for all your code and download this repo into it:

```bash
mkdir ~/code
cd ~/code
git clone https://github.com/dmorales024/vanta.git
```

<!-- AUTHOR: replace the URL above with the real lessons repo URL once it's public. -->

You should see something like:

```
Cloning into 'vanta'...
remote: Enumerating objects: 42, done.
...
Receiving objects: 100% (42/42), done.
```

`git clone` just downloaded the whole repo, every file and its entire history, into a new folder. We'll come back to how that works. For now, open it in VS Code:

```bash
code vanta
```

You can read the rest of the course right there in VS Code, or keep using the browser, whatever you like.

## Done!

That's the hardest chapter. Next up, [the terminal](../01-terminal/).
