# Self-check

<!-- AUTHOR: optional intro line in your voice. -->
If you've used git before, answer these without looking anything up. Everyone still does every chapter, this just tells you where to slow down. Answers are at the bottom.

1. What's the difference between `git add` and `git commit`?
2. You ran `git add` on the wrong file. How do you unstage it without losing your changes?
3. What does `git log --oneline` show, and what's the short string at the start of each line?
4. Which of these is a good commit message? `Fixed the bug.` / `fix: correct shrek release year` / `update`
5. What's the difference between `git switch main` and `git switch -c main`?
6. What's a fast-forward merge?
7. What is a branch, actually, inside `.git`?
8. What's the difference between `git fetch` and `git pull`?
9. You try to `git push` and it's rejected with `fetch first`. What happened and what do you do?
10. Why should you never rebase commits you've already pushed?
11. What do `<<<<<<<` and `>>>>>>>` mean in a file?
12. You ran `git reset --hard` and lost a commit. How do you get it back?

---

## Answers

1. `add` puts changes in the staging area, `commit` saves everything staged as a snapshot. Chapter 2.
2. `git restore --staged <file>`. Chapter 9.
3. One commit per line, newest first. The string is the start of the commit's hash. Chapters 2 and 4.
4. `fix: correct shrek release year`. Chapter 2.
5. `switch main` goes to an existing branch, `-c` tries to create a new one (and fails if it exists). Chapter 3.
6. When the branch you're merging into hasn't moved, so git just slides its name forward with no merge commit. Chapter 3.
7. A small file in `.git/refs/heads/` containing a commit hash. Chapter 4.
8. `fetch` downloads new commits without touching your files, `pull` fetches and then merges. Chapter 5.
9. GitHub has commits you don't. `git pull`, fix any conflicts, then push. Chapters 5 and 7.
10. Rebase makes new commits with new hashes, so anyone who built on the old ones ends up with a history that disagrees with yours. Chapter 8.
11. A merge conflict. Between them are the two versions of the same lines. Chapter 7.
12. `git reflog` to find its hash, then `git reset --hard <hash>`. Chapter 9.

If you got 1 to 6 right, chapters 1 to 3 will be review. Still do them, since your `movies` repo gets built there, but you can move fast. If you missed any of 7 to 12, that's exactly what the rest of the course is for!
