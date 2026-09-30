# 5. GitHub

Everything so far has lived only on your computer. If your laptop dies, your repo dies with it. GitHub is a website that hosts git repos, so you can back yours up, share it, and work on it with other people. Git and GitHub aren't the same thing: git is the tool, GitHub is one place to put repos. 

By the end of this chapter your `movies` repo will be on GitHub and you'll know how to send changes up and pull them back down.

## Part 1: SSH keys

When you push to GitHub, it needs to know it's really you. We're going to use **SSH keys** for that. It's a bit more setup than typing a password, but you do it once per computer, it's more secure, and it's how most developers connect to GitHub.

An SSH key comes in two halves:

- a **private key** that stays on your computer and you never, ever share
- a **public key** that you give to GitHub

When you connect, your computer proves it has the private key that matches the public one GitHub has, without ever sending the private key anywhere.

### Check for an existing key

```bash
ls -al ~/.ssh
```

If you see `id_ed25519` and `id_ed25519.pub`, you already have a key, skip to [Add the key to GitHub](#add-the-key-to-github). If it says `No such file or directory`, keep going.

### Make a key

Use the email from your GitHub account:

```bash
ssh-keygen -t ed25519 -C "you@example.com"
```

It'll ask you three things:

1. **Where to save it.** Just press Enter for the default.
2. **A passphrase.** Type one (or not, I would just press enter so you don't have to type it every time you commit/push). This is a password for the key itself, so if someone gets onto your laptop they still can't use it. Nothing shows up while you type, that's normal. 
3. **The passphrase again.**

You should see something like:

```
Your identification has been saved in /Users/yourname/.ssh/id_ed25519
Your public key has been saved in /Users/yourname/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:AbCdEf... you@example.com
```

`id_ed25519` is the private key. `id_ed25519.pub` is the public one.

### Add the key to GitHub

Copy your **public** key (the `.pub` one!):

| OS | Command |
|---|---|
| Mac | `pbcopy < ~/.ssh/id_ed25519.pub` |
| Windows | `clip < ~/.ssh/id_ed25519.pub` |
| Linux | `cat ~/.ssh/id_ed25519.pub`, then select the output and copy it |

It should start with `ssh-ed25519` and end with your email.

On GitHub: click your profile picture (top right), then **Settings**, then **SSH and GPG keys**, then **New SSH key**. Give it a title like `laptop`, leave the type as **Authentication Key**, paste your key, and click **Add SSH key**.

### Test it

```bash
ssh -T git@github.com
```

The first time, you'll see a warning like this:

```
The authenticity of host 'github.com (140.82.112.3)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU.
Are you sure you want to continue connecting (yes/no/[fingerprint])?
```

Your computer has never talked to GitHub over SSH before, so it's checking. Make sure the fingerprint matches `SHA256:+DiY3wvvV6TuJJhbpZisF/zLDA0zPMSvHdkr4UvCOqU` exactly, then type `yes` (the whole word).

You should see:

```
Hi yourusername! You've successfully authenticated, but GitHub does not provide shell access.
```

That means it worked! The "does not provide shell access" part is normal.

## Part 2: Push your repo

### Make an empty repo on GitHub

On GitHub, click the **+** at the top right, then **New repository**.

- Name it `movies`.
- Public or private, your call.
- **Don't** check "Add a README", don't add a .gitignore or a license. You already have a repo, and GitHub's needs to start empty.

Click **Create repository**. GitHub shows you a setup page. Click the **SSH** button at the top so the URL looks like `git@github.com:yourusername/movies.git`, not `https://...`.

### Connect your repo to it

A **remote** is another copy of your repo that yours knows about. By convention, the main one is called `origin`.

```bash
cd ~/code/movies
git remote add origin git@github.com:yourusername/movies.git
git remote -v
```

You should see:

```
origin	git@github.com:yourusername/movies.git (fetch)
origin	git@github.com:yourusername/movies.git (push)
```

### Push

```bash
git push -u origin main
```

This sends your `main` branch to `origin`. The `-u` connects your local `main` with the one on GitHub, so from now on plain `git push` and `git pull` know where to go.

You should see something like:

```
Enumerating objects: 30, done.
...
To github.com:yourusername/movies.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```

Refresh the page on GitHub. There's your repo! Click on the commits and look around. Every hash matches what `git log` shows you on your laptop, because it's the exact same commits.

## Part 3: Pull changes down

Right now your laptop and GitHub match. Let's make GitHub get ahead.

On GitHub, open `movies.md` and click the pencil icon to edit it. Add a movie at the bottom. Click **Commit changes**, and give it a proper message like `feat: add inception`. Yes, conventional commits on the website too!

Your laptop doesn't know about that commit yet. First, **fetch** it:

```bash
git fetch
git log --oneline --all
```

You should see something like:

```
a9b8c7d (origin/main) feat: add inception
3e4f5a6 (HEAD -> main) feat: add parasite
...
```

`git fetch` downloads new commits but doesn't touch your files. `origin/main` is your laptop's copy of where `main` is on GitHub, and it's one commit ahead of your `main`. Check `cat movies.md`, the new movie isn't there yet.

To bring it into your branch:

```bash
git merge origin/main
```

That was a fast-forward, just like in chapter 3. Now `cat movies.md` shows the new movie.

Doing `fetch` then `merge` is so common that there's one command for both:

```bash
git pull
```

Use `git pull` from now on, but remember that's all it is: fetch, then merge. The `pull.rebase false` setting from chapter 0 is what tells it to merge.

## The everyday loop

This is what working with GitHub looks like most days:

```bash
git pull                      # get the latest first
# ...edit files...
git add .
git commit -m "feat: ..."
git push                      # send it up
```

Get in the habit of pulling *before* you start working. It saves you from a lot of the trouble in chapter 7.

## When things go wrong

| You see | What's wrong | Fix |
|---|---|---|
| `Permission denied (publickey)` | GitHub doesn't recognize your key | Run `ssh-add -l`. If it says no identities, redo the ssh-agent step. Check the key is on GitHub under Settings > SSH and GPG keys. |
| `Could not open a connection to your authentication agent` | ssh-agent isn't running in this terminal | Mac/Linux: `eval "$(ssh-agent -s)"` then `ssh-add`. Windows: check the `.bashrc` code is saved and reopen Terminal. |
| `git push` asks for a username and password | Your remote is an `https://` URL, not SSH | `git remote set-url origin git@github.com:yourusername/movies.git` |
| `Host key verification failed` | You didn't type `yes` at the first-connection question | Run `ssh -T git@github.com` again and type `yes` |
| `ssh -T` just hangs | Some school Wi-Fi blocks SSH | Tell your teacher. There's a workaround using port 443. |
| `! [rejected] main -> main (fetch first)` | GitHub has commits you don't | `git pull`, then `git push` again |
| `fatal: remote origin already exists` | You already added it | `git remote -v` to check it, or `git remote set-url origin <url>` to fix it |

## Your turn

1. Make a branch, add a movie, commit it, and push the branch with `git push -u origin <branch-name>`. Find it on GitHub under the branch dropdown.
2. Merge it into `main` on your laptop, push `main`, and delete the branch locally with `git branch -d`.
3. Edit a rating on GitHub's website with a `fix:` message, then `git pull` it down.

## Recap

| Command | Does |
|---|---|
| `ssh-keygen -t ed25519 -C "email"` | makes an SSH key |
| `ssh -T git@github.com` | tests your SSH connection |
| `git remote add origin <url>` | connects your repo to GitHub |
| `git remote -v` | lists remotes |
| `git push -u origin main` | first push of a branch |
| `git push` | sends commits up |
| `git fetch` | downloads commits without changing your files |
| `git pull` | fetch + merge |
| `git clone <url>` | downloads a whole repo (you did this in setup!) |

One last core chapter: [ignoring files](../06-gitignore/).
