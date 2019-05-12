* Comparison
  ----------

    http://hyperpolyglot.org/unix-shells
      exhaustive feature comparison between different unix shells

    http://www.zsh.org/zsh/FAQ
      some details on the features of zsh and some comparison with bash

    http://grml.org/zsh/zsh-lovers
      A collection of tops and tricks for zsh


* brace expansion
  ---------------

    ➜  ~  bash -c 'echo {a..z}'
    a b c d e f g h i j k l m n o p q r s t u v w x y z
    ➜  ~  bash -c 'echo {0..9}'
    0 1 2 3 4 5 6 7 8 9
    ➜  ~  echo {0..9}
    0 1 2 3 4 5 6 7 8 9
    ➜  ~  echo {a..z}
    {a..z}
    ➜  ~  echo {0-9}
    {0-9}
    ➜  ~  echo {a-z}
    {a-z}
    ➜  ~  setopt BRACE_CCL
    ➜  ~  echo {0-9}
    0 1 2 3 4 5 6 7 8 9
    ➜  ~  echo {a-z}
    a b c d e f g h i j k l m n o p q r s t u v w x y z

* Intro to ZSH
  ------------
    http://zsh.sourceforge.net/Intro/intro_toc.html
      features and intro to zsh

    http://www.bash2zsh.com/zsh_refcard/refcard.pdf

