# parent-repo

This repo is only a showcase on how to import an external git repo ([source-repo](https://github.com/octaviobr/source-repo)) with it's history into a sub folder (`/infra`)

### How to Import a Git Repository into a Subfolder of Another Git Repository
To import a Git repository (including its history) into a folder within another Git repository, follow these steps. These steps ensure the full commit history of the imported repository is preserved while embedding it in a subfolder of the target repository.

---
### Steps to Import a Git Repository into a Subfolder
1. **Clone both repositories**:
   ```bash
   git clone git@github.com:OctavioBR/source-repo.git
   git clone git@github.com:OctavioBR/parent-repo.git
   ```
2. **Reorganize the source repository into a subfolder**
   ```bash
   cd source-repo
   git filter-repo --to-subdirectory-filter infra
   ```
   > **note**: The source repo has to be a **fresh clone** otherwise `filter-repo` will fail with "Refusing to destructively overwrite repo history since this does not look like a fresh clone."<br>
   > If you don't have `git filter-repo`, you can install it (preferred over git filter-branch for performance and maintenance reasons): `pip install git-filter-repo`
3. **Add the source repository as a remote in the parent repo and fetch**:
   ```bash
   cd ../parent-repo
   git remote add source-repo ../source-repo
   git fetch source-repo
   ```
4. **Merge the rewritten branch into the target repository**:
   ```bash
   git merge --allow-unrelated-histories source-repo/main
   ```
   > The `--allow-unrelated-histories` flag is necessary because the two repositories do not share a common commit history.
5. **Clean up (optional)**: Remove the remote reference with:
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
If everything looks good, push your changes to the remote repository.
