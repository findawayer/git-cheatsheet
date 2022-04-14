# Git-cheatsheet

## Auth

### Generate new SSH key

```
ssh-keygen -t ed25519 -C "email@example.com"
```

### Authenticate with generated SSH key

```
eval $(ssh-agent -s)
ssh-add ~/.ssh/id_rsa
ssh -T git@github.com
```

1. Launch ssh-agent in the background
1. Add generated SSH key.
1. Try to SSH to GitHub.

## Configuration

- `git config --global`: Define something globally.
- `git config`: Define something for current project.
- `git config alias`: Define an alias for a specific command.
  - e.g. `git config --global alias.unstage 'reset HEAD --'`
- `git config --global credential.helper cache`: Remember credential for a while.
- `git config --global pull.rebase true`: Rebase when pull.

## New repo

```
echo "# git-test" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M master
git remote add origin git@github.com:<username>/<repository>.git
git push -u origin master
```

1. Create new repo on GitHub.
1. Create README.md file.
1. Start using git.
1. Add the REAMDE file to the index.
1. Commit with "first commit" message.
1. Move(forced) to master branch.
1. Add remote branch using SSH or HTTPS.
1. Push the commit to the upstream.

## Clone existing repo

- `git clone <respository>`: Create a new folder and clone the repo there.

## Manage remotes

- `git remote`: Show remote branches.
  - `-v`: Show with URL.
- `git remote add <branch>`: Add a remote.
- `git remote set-url <branch>`: Change remote target.
- `git remote rename <from> <to>`: Rename a remote.
- `git remote remove <branch>`: Delete a remote.

## Managing branches

- `git branch`: Show current branch.
  - `<branch>`: Make a new branch named `<branch>`.
  - `-a`: List all branches.
  - `-v`: List branches with the latest commit.
  - `-d <branch>`: Delete the branch.
  - `-m <from> <to>`: Rename a branch.
  - `--merged`: List branches merged into HEAD.
  - `--no-merged`: List branches not merged into HEAD.
- `git checkout <branch>`: Move to `<branch>`.
  - `-b <branch>`: Make a new branch named `<branch>` and move there.
  - `-b <branch> origin/<remote>`: Make a new branch out of source in `<remote>` branch.
- `git merge <branch>`: merge `<branch>` to master.

## Making changes

- `git add <file>`: Add a changed file to the index.
  - `./*/-a`: Add all changed files to the index.
- `git commit -m <message>`: Commit the staged snapshot.
  - `-a -m <message>`: Add to index and commit.
- `git status`: Show staged/unstaged/untracked files.
- `git log`: Display commit history.
  - `--pretty=oneline/short/full/fuller`: Pretty format.
- `git diff`: Show changes between my index and working directory.
- `git mv <from> <to>`: Rename a file.
- `git rm <file>`: Remove a file both from the index & working directory.

## Fixing changes

- `git commit --amend`: Replace the last commit with the staged changes.
  - This is equivalent to `git reset --soft HEAD~` then `git commit`
  - `-m <message>`: Replace also the last commit message.

## Undoing changes

- `git revert <commit>`: Create new commit that undoes changes from `<commit>`.
- `git reset <file>`: Undo commit and unstage files in index.
  - `--soft <file>`: Undo commit and but keeps the changes in index.
  - `--mixed <file>`: Undo commit and unstage files in index.
  - `--hard <file>`: Undo commit, unstage files and reset working directory.
- `git reset <commit>`: Reset HEAD to `<commit>`
  - `--soft <commit>`: Reset the index(Undo not pushed commits) to `<commit>`.
  - `--mixed <commit>`: Reset the index but not the working directory.
  - `--hard <commit>`: Reset the index and working directory.
  - `--merge <commit>`: Reset the index and merge files between `<commit>` and `HEAD`.
  - `--keep <commit>`: Reset the index and merge files while keeping local changes.

## Submitting changes

- `git push <remote>`: Send changes in the local `<branch>` to the `<remote>` repo.
  - `--tags`: Push along with tags.
  - `--all`: Push all local branches including tags to the `<remote>` repo.
  - `git push -u origin <branch>:<remote>`: Push local `<branch>` to remote `<remote>` branch, setting it as upstream.
  - `--delete <remote>`: Delete a remote branch.

## Keep up with changes

- `git fetch <branch>`: Get changes and refs from `<branch>`.
  - `git fetch origin`: Sync with remote.
- `git pull <branch>`: Fetch and merge with `<branch>`.
  - `--rebase`: Fetch, merge and rebase.

## Merging changes

- `git merge <from> <to>`: Merge a branch to another.
  - `--sqaush`: Summerize all changes.

## Pull Request

- `git request-pull origin/master <branch>`: Create a PR from `<branch>` to remote master.

```
$ git checkout -b featureB origin/master
# (Working...)
$ git commit
$ git push myfork featureB
$ git request-pull origin/master myfork
# (Email generated request pull to maintainer...)
$ git fetch origin
# (The maintainer encounters conflicts, rebase it again into local branch)
$ git checkout featureA
$ git rebase origin/master
$ git push -f myfork featureA
# ()
```

## Reflog

- `git reflog`: Find references.

## Useful selectors

- `HEAD`: Current local branch.
- `HEAD~N`, `HEAD^N`
  - `~2` means up two levels in the hierarchy.
  - `^2` means the second parent.
  - Read: [whats-the-difference-between-head-and-head-in-git](https://stackoverflow.com/questions/2221658/whats-the-difference-between-head-and-head-in-git)
- `--cached, --staged`: Only the index. (diff, rm...)
- `<commit>..<commit>`: Select all commits between the specified 2.
