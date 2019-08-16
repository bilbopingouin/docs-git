# Sed


<!-- vim-markdown-toc GFM -->

* [Replace a pattern in several files](#replace-a-pattern-in-several-files)
* [Extract specific lines from a file](#extract-specific-lines-from-a-file)

<!-- vim-markdown-toc -->

### Replace a pattern in several files
  
Using `find`:

```bash
find . -type f \( -name "*.h" -or -name "*.c" \) -exec sed -i 's/pattern/replacement/' {} \;
```
    
The `-i` stands for in-place.

### Extract specific lines from a file

```bash
sed -n '5,10p;11q' file
```

It is also possible to use
```bash
sed -n '\cpatternc,10p;11q' file
```


