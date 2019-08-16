# Shell


<!-- vim-markdown-toc GFM -->

* [Comparison](#comparison)
* [brace expansion](#brace-expansion)
* [Intro to ZSH](#intro-to-zsh)

<!-- vim-markdown-toc -->

### Comparison


- exhaustive feature comparison between different unix shells [Unix Shells](http://hyperpolyglot.org/unix-shells),
- some details on the features of zsh and some comparison with bash [zsh FAQ](http://www.zsh.org/zsh/FAQ),
- A collection of tops and tricks for zsh [zsh lovers](http://grml.org/zsh/zsh-lovers).


### brace expansion

```bash
$ bash -c 'echo {a..z}'
  a b c d e f g h i j k l m n o p q r s t u v w x y z
$ bash -c 'echo {0..9}'
  0 1 2 3 4 5 6 7 8 9
$ echo {0..9}
  0 1 2 3 4 5 6 7 8 9
$ echo {a..z}
  {a..z}
$ echo {0-9}
  {0-9}
$ echo {a-z}
  {a-z}
$ setopt BRACE_CCL
$ echo {0-9}
  0 1 2 3 4 5 6 7 8 9
$ echo {a-z}
  a b c d e f g h i j k l m n o p q r s t u v w x y z
```

### Intro to ZSH

- features and intro zsh [intro toc](http://zsh.sourceforge.net/Intro/intro_toc.html)
- reference card [refcard](http://www.bash2zsh.com/zsh_refcard/refcard.pdf)

