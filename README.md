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

