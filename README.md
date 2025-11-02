A bunch of useful bash commands.

# Cheat sheet

Profile with `perf`.

```bash
perf record -g ./a.out
perf report -g graph,0.5,caller
```

List installed paquets.

```bash
dpkg --get-selections > .paquets
```

Add user to group sudo.

```bash
usermod -a -G sudo <user>
```


## `find`

Larger than 1MB.

```bash
find . -type f -size +1M
```

At most 5 days old (modification time).

```bash
find . -type f -newermt '-5 days'
```

Replace tabs by spaces in a project.

```bash
mkdir tmp
find . -name "*.cpp" -type f -exec bash -c 'expand -t 4 "$0" > tmp/expanded && mv tmp/expanded "$0"' {} \;
rm -R tmp
```

Count lines of sources files in project.

```bash
find src -name *.[hc]pp -print0 | xargs -0 wc -l
```

Find instances of "foo" in a project and print the filename/line numbers.

```bash
find src -name *.[hc]pp -exec grep -H -n "foo" {} \;
```

Replace "foo" by "bar" in a project.
Pro tip: sed accepts any delimiter character.

```bash
find src -name *.[hc]pp -exec sed -i "s#foo#bar#g" {} \;
```

Set good default permissions after transfer through non-Unix filesystem.

```bash
find . -type f -exec chmod 644 {} \;
find . -type d -exec chmod 755 {} \;
```

Compress PDF files larger than 1MB.

```bash
find . -type f -name "*.pdf" -size +1M -exec xz --verbose {} \;
```

Sum sizes of all PDF files.

```bash
# "tr" merges delimiters together.
# The 5th column contains the file size.
# "paste -d+" concatenates lines with a "+" delimiter.
# "bc" calculates the input.
find . -type f -name "*.pdf" -exec ls -l {} \; | tr -s ' ' | cut -d' ' -f 5 | paste -sd+ | bc
```


## `git`

Squash all git commits.

```bash
git reset $(git commit-tree HEAD^{tree} -m "Initial commit")
```

Diff directories.

```bash
git diff --no-index foo/ bar/
```

Sync with upstream repo.

```bash
git remote -v
git remote add <remote-name> <url>
git fetch <remote-name>
git rebase <remote-name>/master
git push
git branch -d fix-***
```

Create pull request.

```bash
git checkout -b fix-***
git commit ...
git push --set-upstream origin fix-***
```

Fetch branch from contributor.

```bash
git remote add <remote-name> <url>
git remote add -t <branch> <remote-name>/<branch>
git checkout <branch>
# Alternatively
git checkout --track <remote-name>/<branch>
```

Make a local branch track a remote branch.

```bash
git branch --track <branch> <remote-name>/<branch>
```

Prune branches that have been removed remotely.

```bash
git fetch --prune
```

Clone with submodules.

```bash
git clone --recurse-submodules https://github.com/USERNAME/REPOSITORY
```

Sync submodules to fetched version on main repo.

```bash
git fetch
git rebase
git submodule status
git submodule update
```

Add tags.

```bash
git tag -a <tag> <commit>
git push --follow-tags
```

Pull all branches.

```bash
for remote in `git branch -r -l origin/*`; do git checkout --track remotes/$remote; done
```

Push all branches (see https://stackoverflow.com/questions/6865302/push-local-git-repo-to-new-remote-including-all-branches-and-tags).

```bash
git push origin --all
git push origin --tags
```

### Configuration

Don't save my password thanks.

```bash
git config --global --add credential.helper ""
```

Set editor properly for commit messages.

```bash
git config --global core.editor $(which vim)
```

Manually edit config.

```bash
git config --global --edit
```

Useful aliases (see https://stackoverflow.com/questions/1057564/pretty-git-branch-graphs).

```
alias.lg=log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all
```

Setup SSH authentication.

```bash
git remote set-url --push origin git@github.com:USERNAME/REPOSITORY.git
git remote set-url --push origin git@codeberg.org:USERNAME/REPOSITORY.git
git remote set-url --push origin git@gitlab.com:USERNAME/REPOSITORY.git
```


## `apt`

Get rid of `The following packages have been kept back`.

```bash
apt-get --with-new-pkgs upgrade
```

