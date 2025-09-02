The command `git rebase --onto temp <hash-of-parent-of-your-new-commit> <hash-of-your-new-commit>` is a Git rebase operation that moves a specific commit (or range of commits) to a new base, in this case, the tip of the `temp` branch. Let’s break it down step-by-step to explain what it does:

### Command Breakdown
- **`git rebase --onto <new-base> <upstream> <branch-or-commit>`**:
  - `--onto <new-base>`: Specifies the new base where the commits will be applied. In this case, `temp` is the target branch (its latest commit).
  - `<upstream>`: Defines the point up to which commits are excluded from being rebased. Here, `<hash-of-parent-of-your-new-commit>` is the parent commit of the commit you want to move.
  - `<branch-or-commit>`: Specifies the commit (or range of commits) to rebase. Here, `<hash-of-your-new-commit>` is the commit you want to move.

### What It Does
This command takes the commit identified by `<hash-of-your-new-commit>` (and only that commit, since it’s a single commit) and "replays" it on top of the latest commit in the `temp` branch. The `<hash-of-parent-of-your-new-commit>` is used to exclude all commits up to that point, ensuring only the specified commit is moved.

In simpler terms:
- It picks your new commit (the one you created after checking out an older commit).
- It applies that commit on top of the `temp` branch’s latest commit.
- The result is a new commit (with a different hash) that contains the same changes but is now based on the tip of `temp`.

### Example Scenario
Let’s say the Git history looks like this:
```
C (temp) - Latest commit on temp branch
B - Older commit
A - Even older commit
```
You checked out commit `A`, made changes, and created a new commit `D`. The current state is:
```
D (HEAD) - Your new commit
A - Older commit
```
Meanwhile, `temp` is still pointing at `C`. You want to move `D` to be on top of `C`.

You run:
```bash
git rebase --onto temp A D
```
- `temp`: The new base (points to commit `C`).
- `A`: The parent of `D`, so Git excludes everything up to `A`.
- `D`: The commit you want to rebase.

Git will:
1. Take the changes introduced in commit `D`.
2. Apply those changes on top of the tip of `temp` (commit `C`).
3. Create a new commit, say `D'`, with the same changes as `D` but based on `C`.

The resulting history will look like:
```
D' (HEAD) - Your new commit (rebased)
C (temp) - Latest commit on temp
B - Older commit
A - Even older commit
```

### Key Points
- **Preserves Changes**: The content of your new commit (`D`) is preserved, but it gets a new commit hash (`D'`) because it’s now based on a different parent (`C` instead of `A`).
- **Single Commit**: Since you specified `<hash-of-your-new-commit>`, only that commit is rebased. If you had specified a branch or a range, multiple commits could be moved.
- **Conflicts**: If the changes in `D` conflict with changes in `C`, Git will pause and ask you to resolve conflicts. You’d resolve them, then run `git rebase --continue`.
- **Branch Update**: After rebasing, `D'` is not automatically part of the `temp` branch. You need to update the `temp` branch to point to `D'` by running:
  ```bash
  git checkout temp
  git reset --hard <hash-of-D'>
  ```

### When to Use This
This command is useful when you’ve created a commit based on an older point in the history (like `A`) but want to move it to the latest state of a branch (like `temp`). It’s a clean way to reorganize your commits without manually reapplying changes.

If you need further clarification or an example tailored to your specific Git history, let me know!