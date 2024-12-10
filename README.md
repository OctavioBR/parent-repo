# parent-repo

This repo is only a showcase on how to import an external git repo ([source-repo](https://github.com/octaviobr/source-repo)) with it's history into a sub folder (`/infra`)

### How to Import a Git Repository into a Subfolder of Another Git Repository

To import a Git repository (including its history) into a folder within another Git repository, follow these steps. These steps ensure the full commit history of the imported repository is preserved while embedding it in a subfolder of the target repository.

---

### Steps to Import a Git Repository into a Subfolder

1. **Navigate to the target repository**:
   ```bash
   cd /path/to/target-repository
   ```

2. **Add the source repository as a remote**:
   ```bash
   git remote add source-repo <URL-or-path-to-source-repo>
   ```

3. **Fetch the source repository**:
   ```bash
   git fetch source-repo
   ```

4. **Create a branch for the source repository**:
   ```bash
   git checkout -b source-repo-branch source-repo/main
   ```
   Replace `main` with the default branch of the source repository if it is not `main`.

5. **Reorganize the source repository into a subfolder**:
   Use `git filter-repo` to rewrite the commit history of the source repository to be inside a subfolder. If you don't have `git filter-repo`, you can install it (preferred over `git filter-branch` for performance and maintenance reasons):
   ```bash
   pip install git-filter-repo
   ```
   Then, run the following:
   ```bash
   git filter-repo --to-subdirectory-filter <folder-name>
   ```
   Replace `<folder-name>` with the name of the folder where you want to import the source repository.

6. **Switch back to the target repository's branch**:
   ```bash
   git checkout main
   ```

7. **Merge the rewritten branch into the target repository**:
   ```bash
   git merge --allow-unrelated-histories source-repo-branch
   ```
   The `--allow-unrelated-histories` flag is necessary because the two repositories do not share a common commit history.

8. **Clean up (optional)**:
   - Remove the temporary branch:
     ```bash
     git branch -d source-repo-branch
     ```
   - Remove the remote reference:
     ```bash
     git remote remove source-repo
     ```

---

### Explanation of What Happens
- The `git filter-repo --to-subdirectory-filter` command rewrites the history of the imported repository, placing all its files and commits into the specified subdirectory.
- The `git merge` step combines the two histories while maintaining the full commit history of both repositories.

### Final Verification
Check your repository to ensure the files and history are correctly imported:
```bash
git log --oneline --graph --all
```

If everything looks good, commit and push your changes to the remote repository.
