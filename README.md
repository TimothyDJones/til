# Today I Learned (TIL) - Personal Knowledge Base

Brief tips and tricks from various places.

## [Docker Tips/Shortcuts](https://github.com/TimothyDJones/til/blob/main/Docker_Tips.md)
## [Python Tips/Shortcuts](https://github.com/TimothyDJones/til/blob/main/Python_Tips.md)
## [Tmux Tips/Shortcuts](https://github.com/TimothyDJones/til/blob/main/Tmux_Tips.md)
## [Vim Tips/Shortcuts](https://github.com/TimothyDJones/til/blob/main/Vim_Tips.md)
## [VS Code Tips/Shortcuts](https://github.com/TimothyDJones/til/blob/main/VS_Code_Tips.md)

## Git

### Reset branch to remote (forcefully)
This example uses the `main` branch.  Replace `main` with desired branch.
```bash
git fetch origin
git reset --hard origin/main
git clean -f
```
To see which files will be removed by [`clean`](https://git-scm.com/docs/git-clean) **without** removing them, run:
```bash
git clean -n -f
```

[Reference](https://stackoverflow.com/a/27664932)

### Merge branch and remove from local and remote
After completing work on a branch, you can merge back to the `main` branch and remove it from both the local and remote. This example uses the working branch named `working_branch`.
```bash
git checkout main
git pull origin main
git merge working_branch
# Fix any merge conflicts, as necessary.
git push -u origin main
git branch -d working_branch				# Delete local branch
git push origin --delete working_branch		# Delete remote branch
```

### Add tags to local and remote
Git [tags](https://initialcommit.com/blog/git-tag) are useful for maintaining check points of milestones in the development process. Typically, [semantic versioning](https://semver.org/) is recommended for tag numbers. To apply tags:
```bash
git checkout main								# Ensure that you are on main branch.
git tag -a vX.Y.Z -m "Annotation for this tag."	# Semantic verison tag for version X.Y.Z to local (on current branch).
git push --tags									# Apply tag to remote.
git ls-remote --tags							# Display tags on remote.
```