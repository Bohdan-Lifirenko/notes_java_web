To add your new commit to the end of the `temp` branch after checking out an older commit, you can use **git rebase** to replay your new commit onto the latest state of the `temp` branch. Here’s a step-by-step guide:

1. **Identify the current state**: You’ve checked out an older commit in the `temp` branch, made changes, and committed them. Your new commit is now at the current HEAD, but it’s based on the older commit, not the tip of the `temp` branch.

2. **Find the commit hashes**:
   - Run `git log --oneline` to see the commit history. Note the hash of your new commit (the latest one) and the hash of the tip of the `temp` branch (the commit you want to append your new commit to).

3. **Reset to the tip of the temp branch**:
   - Run `git checkout temp` to ensure you’re on the `temp` branch.
   - If you’re not already at the tip of the branch, run `git reset --hard <hash-of-temp-branch-tip>` to move to the latest commit on `temp`. Replace `<hash-of-temp-branch-tip>` with the hash of the latest commit on the `temp` branch.

4. **Cherry-pick your new commit**:
   - Use `git cherry-pick <hash-of-your-new-commit>` to apply your new commit on top of the current tip of the `temp` branch. Replace `<hash-of-your-new-commit>` with the hash of the commit you created.

5. **Resolve conflicts (if any)**:
   - If there are conflicts during the cherry-pick, Git will pause and let you resolve them. After resolving conflicts, run:
     ```bash
     git add .
     git cherry-pick --continue
     ```

6. **Verify the history**:
   - Run `git log --oneline` to confirm that your new commit is now at the tip of the `temp` branch.

7. **Push the changes (if needed)**:
   - If the `temp` branch is tracked remotely and you want to update the remote branch, run:
     ```bash
     git push --force
     ```
     **Note**: Use `--force` with caution, as it overwrites the remote branch. If others are working on the same branch, consider using `git push --force-with-lease` to avoid overwriting their changes.

### Alternative Approach: Rebase
If you want to keep the history cleaner or have multiple commits to move, you can use **rebase** instead of cherry-picking:

1. **Check out your new commit**:
   - If you’re still on the commit you created, note its hash. Otherwise, check it out with `git checkout <hash-of-your-new-commit>`.

2. **Rebase onto the temp branch**:
   - Run:
     ```bash
     git rebase --onto temp <hash-of-parent-of-your-new-commit> <hash-of-your-new-commit>
     ```
     - `<hash-of-parent-of-your-new-commit>` is the commit just before your new commit (the older commit you checked out).
     - This moves your new commit to the tip of the `temp` branch.

3. **Update the temp branch**:
   - After rebasing, run:
     ```bash
     git checkout temp
     git reset --hard <hash-of-rebased-commit>
     ```
     Replace `<hash-of-rebased-commit>` with the hash of your new commit after rebasing.

4. **Push the changes**:
   - As above, use `git push --force` or `git push --force-with-lease` if needed.

### Example Scenario
Suppose your `temp` branch history looks like this:
```
C (HEAD, temp) - Latest commit on temp
B - Older commit
A - Even older commit
```
You checked out commit `A`, made changes, and created a new commit `D`. Now your history looks like:
```
D (HEAD) - Your new commit
A - Older commit
```
Meanwhile, `temp` is still pointing at `C`. To add `D` to the end of `temp`:
- Run `git checkout temp` to go to `C`.
- Run `git cherry-pick <hash-of-D>` to apply `D` on top of `C`.
- The new history will be:
  ```
  D' (HEAD, temp) - Your new commit (rebased)
  C - Latest commit on temp
  B - Older commit
  A - Even older commit
  ```

If you encounter issues or need clarification, let me know!