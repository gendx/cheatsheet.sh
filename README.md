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

