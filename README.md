A bunch of useful bash commands.

# Cheat sheet

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

Set good default permissions after transfer through non-Unix filesystem.

```bash
find . -type f -exec chmod 644 {} \;
find . -type d -exec chmod 755 {} \;
```

Compress PDF files larger than 1MB.

```bash
find . -type f -name "*.pdf" -size +1M -exec xz --verbose {} \;
```

Profile with `perf`.

```bash
perf record ./a.out
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

At most 5 days old.

```bash
find . -type f -newermt '-5 days'
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
git remote add <name> <url>
git fetch upstream
git rebase upstream/master
git push
git branch -d fix-***
```

Create pull request.

```bash
git checkout -b fix-***
git commit ...
git push --set-upstream origin fix-***
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

Don't save my password thanks.

```bash
git config --global --add credential.helper ""
```

Set editor properly for commit messages.

```bash
git config --global core.editor $(which vim)
```

Setup SSH authentication.

```bash
git remote set-url --push origin git@github.com:USERNAME/REPOSITORY.git
```

# `apt`

Get rid of `The following packages have been kept back`.

```bash
apt-get --with-new-pkgs upgrade
```

