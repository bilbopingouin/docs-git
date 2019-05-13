# delete all empty directories

```bash
find . -type d -empty -delete
```

# look for soft links

```bash
find . -type l
```

# Exclude path

```bash
for file in `find . -type f -not -path "*.git/*"`; do diff -q $file ~/.$file; done
```

