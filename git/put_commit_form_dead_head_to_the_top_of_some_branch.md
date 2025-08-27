Got it 👍 Let’s clarify the situation step by step:

1. You had branch **`temp`**.
2. You checked out an **older commit** inside it (so you were in a detached HEAD state).
3. You made changes and committed them → now you have a **new commit**, but not on top of `temp`.
4. You want to **move this new commit to the end of `temp`** (the latest tip).

---

### ✅ Solution: Cherry-pick

Since your new commit is "off to the side," the cleanest way is to **cherry-pick** it onto the branch tip.

```bash
# Make sure you’re on temp branch latest
git checkout temp

# Cherry-pick the new commit
git cherry-pick <your_new_commit_hash>
```

Now your new commit is appended at the end of `temp`.

---

### ✅ Alternative: Rebase

If you actually want to "move" history (like your new commit should logically replace the old one), you could do:

```bash
# From your new commit
git rebase temp
```

This re-applies your new commit on top of the latest `temp`.
(If you’re not currently on the new commit, first `git checkout <new_commit_hash>` before rebase.)

---

### ✅ If you want to **fast-forward branch temp** to the new commit

If you decide that your new commit should *become* the tip of `temp` (instead of keeping existing new commits), then you can force-move the branch:

```bash
git branch -f temp <new_commit_hash>
git checkout temp
```

This discards commits that were after the old base (careful!).

---

👉 Which one do you prefer: **keeping existing latest commits** on `temp` (so your commit gets added at the end), or **replacing** them with your new commit?
