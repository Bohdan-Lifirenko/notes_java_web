Yes, you can set specific files in your project to the state they had in an older commit while remaining on your current commit (or branch). This process involves restoring the content of those files from a previous commit without changing your current branch or commit history. Here's how to do it using Git:

### Steps to Set Files to a Previous Commit's State
1. **Identify the Commit**:
   Find the commit hash that contains the desired version of the files. Run:
   ```bash
   git log --oneline path/to/your/file
   ```
   Replace `path/to/your/file` with the path to the file(s) you want to restore. Note the commit hash (e.g., `abc123`) where the file(s) were in the desired state.

2. **Restore Specific Files from the Old Commit**:
   Use the `git checkout` or `git restore` command to update the specific files to their state in the chosen commit:
   ```bash
   git checkout abc123 -- path/to/file1 path/to/file2
   ```
   Or, with newer Git versions:
   ```bash
   git restore --source=abc123 -- path/to/file1 path/to/file2
   ```
   - `abc123` is the commit hash.
   - `path/to/file1 path/to/file2` are the files you want to restore (you can list multiple files).
   - This updates the specified files in your working directory and stages them for a commit.
   - Your current branch and HEAD remain unchanged; only the specified files are modified.

3. **Verify the Changes**:
   Check the file contents to confirm they match the state from the old commit:
   ```bash
   cat path/to/file1
   ```
   or open the files in your editor.

4. **Commit the Changes**:
   Since `git checkout` or `git restore` stages the files, you can create a new commit with the restored file versions:
   ```bash
   git commit -m "Restored file1 and file2 to version from commit abc123"
   ```

5. **(Optional) Push to Remote**:
   If you’re working with a remote repository, push the new commit:
   ```bash
   git push origin your-current-branch
   ```

### Important Notes
- **Current Commit Stays the Same**: This process doesn’t move your HEAD or change your current branch. It only modifies the specified files in your working directory and stages them for a new commit.
- **Multiple Files**: You can restore multiple files at once by listing them after the `--` in the `git checkout` or `git restore` command.
- **No History Rewrite**: This creates a new commit with the restored file versions, preserving the existing commit history.
- **Uncommitted Changes**: If the files you’re restoring have uncommitted changes, Git will warn you. Either commit or stash these changes first:
   ```bash
   git stash
   ```
   Then restore the files and apply the stash later if needed:
   ```bash
   git stash pop
   ```

### Example
Suppose you’re on the `main` branch, and you want to restore `src/script.js` and `docs/README.md` to their state from commit `abc123`:
```bash
git checkout abc123 -- src/script.js docs/README.md
git commit -m "Restored script.js and README.md to version from abc123"
```

This keeps you on the `main` branch, updates only those files to their state in `abc123`, and creates a new commit with those changes.

If you encounter issues or need clarification (e.g., handling merge conflicts or specific Git versions), let me know!