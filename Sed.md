# Replace a pattern in several files
  
Using `find`:

    find . -type f \( -name "*.h" -or -name "*.c" \) -exec sed -i 's/pattern/replacement/' {} \;
    
The `-i` stands for in-place.
