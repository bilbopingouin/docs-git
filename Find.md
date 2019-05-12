* delete all empty directories
  ----------------------------

    find . -type d -empty -delete

* look for soft links
  -------------------

    find . -type l

* Exclude path
  ------------

    for file in `find . -type f -not -path "*.git/*"`; do diff -q $file ~/.$file; done

