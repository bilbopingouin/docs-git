# table with header

  
    cat /var/log/fail2ban.log | grep Ban | awk 'BEGIN{print "Day\t\tTime\t\tIP"} {print $1,"\t",$2,"\t",$7}'

prints a table like

    Day         Time    IP
    xxxx.xx.xx  xx:xx   xxxx.xxxx.xxxx.xxxx
    
# split files

  
- split file depending on the first element' value (sorting)

      awk -F, '{print > $1}' file1
    
- same as previous, but changing output file name

      awk -F, '{print > $1".txt"}' file1
    
- sorting and removing the first item (used as filename)

      awk -F, '{print $2 > $1".txt"}' file1
    
- split depending on the values of the second item (compared to 500)

      awk -F, '{if($2<=500)print > "500L.txt";else print > "500G.txt"}' file1
    
- split at every occurence of pattern START

      awk '/START/{x="F"++i;}{print > x;}' file2
    
- same as before but additionally eliminating the pattern's line

      awk '/START/{x="F"++i;next}{print > x;}' file2
    
- same as previous but a header is inserted instead of the pattern

      awk '/START/{x="F"++i;print "ANY HEADER" > x;next}{print > x;}' file2
    
- split every 3rd line

      awk 'NR%3==1{x="F"++i;}{print > x}'  file3
    
- same as before but removing 1st and last line

      sed '1d;$d;' file4 | awk 'NR%3==1{x="F"++i;}{print > x}' 
    
- split every 3rd line with header and trailer kept

      awk 'BEGIN{getline f;}NR%3==2{x="F"++i;a[i]=x;print f>x;}{print > x}END{for(j=1;j<i;j++)print> a[j];}' file4
    

See more details on [awk 10 examples to split file](http://www.theunixschool.com/2012/06/awk-10-examples-to-split-file-into.html).
