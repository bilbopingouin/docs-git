
* tree
  ----
    tree -H title -T title --nolinks -o tree.html
    generates a tree.html file where the output of tree is shown. Useful to print!
    Problem is that to print, it can be pretty complicated, tree > html, html> pdf, etc.
    and it requires a web browser.
    Alternatively I tried
    tree | a2ps -o tree.ps
    but the format was wrong. It should be tried, as it depends on the settings.
    One can also try with the -A and -S options of tree. Currently, the solution is
    LC_ALL=C tree | a2ps -o tree.ps
    where the graphics output is using standard characters.
    
    tree -I "*.o|*.bak" -H PM_S25 -T PM_S25 -o tree.html
    exclude some patterns. Each pattern is separated with a |

* fortune
  -------
    fortune
    fortune -f lists all the files
    fortune file
    fortune 90% file1 file2
    create a new file:
    echo "bla\n%\nblop\n%\nblip" > teststrfile testcp test* /usr/share/games/fortunes/

* ls
  --
    see year in ls -l
    ls -l --full-time

    colors:
    ls --color=auto
    controled by the dir_colors, one can see the defaults in
    dircolors -p
    one can use it to generate a configuration file:
    dircolors -p > ~/.dircolors

    one should note that ls --color or ls -l are trying to get to the linked objects. If that object is not available, ls may hang. run /bin/ls if an alias is defined.
    http://aplawrence.com/Linux/linux_dir_colors.html

* image viewer call from CLI
  --------------------------
    gnome default: eog (eye of gnome)
    typical image viewer: feh
    lightweight GTK: mirage / gqview
    ascii: cacaviewer (caca-utils)

* at                                                                                                                                                                                                            
  --
    example:
    at 13:56at> touch ~/aat> <EOF>job 4 at Mon Jan  5 08:40:00 2015

    One can also add '-m' to get a mail when the job is done
    The commands can be stored in a file and called with
    at HH:MM -f file

    at -l or atq #list jobs
    at -d id or atrm id # delete job with id (4 in previous example)

    One can specify more complex time
    at now + 10 minat noonat 3pmat 2am tomorrow
    ...

    http://www.brunolinux.com/02-The_Terminal/The_at_Command.html
    http://linux.about.com/library/cmd/blcmdl1_at.htm

* elinks shortcuts
  ----------------
    g       -- goto box

    <Right> -- follow link
    <Left>  -- back in history
    h       -- history
    <Up>    -- Previous link
    <Down>  -- Next link

    <Space> -- Next page (buffer)

    <PageDown>/<PageUp>
    <Pos1>/<End>
    <Ins>/<Del>         -- 'scroll' throughout the page (vertically)

    [ ]     -- 'scroll' throughout the page horizontally

    q       -- quit
    /       -- search
    n       -- next occurence

    o       -- options
    <Esc>   -- back from options

    c       -- close tab
    t       -- open a new tab
    T       -- open link in a new tab
    <       -- switch to left tab
    >       -- switch to right tab
    Alt-<   -- move tab to left
    Alt->   -- move tab to right

    http://zeth.net/archive/2007/07/25/command-the-web-an-elinks-tutorial/
    http://elinks.or.cz/documentation/manual.html

* xargs
  -----
    basic usage
    echo 1 2 3 4 | xargs [echo]
    split arguments
    echo 1 2 3 4 | xargs -n 2 [echo]
    without further commands, xargs calls echo

    some commands
    -0 / --null # indicates that arguments ends with 0 instead of whitespace
    e.g.
    find Downloads -type f -print0 | xargs -0 ls
    it allows to treat files with WS in their path/name
    -d '\0' / --delimiter='\0' # would lead to the same result, but using -d, one can choose other delimiter

    -I pattern # offers possibility to use the given pattern
    find . -maxdepth 1 -type d -print0 | xargs -0 -I dir ls dir
    echo 1 2 3 4 | xargs -I file touch file # creates a file ./1\ 2\ 3\ 4
    echo 1 2 3 4 | xargs touch # creates 4 files ./1 ./2 ./3 ./4
    echo 1 2 3 4 5 6 | xargs -n 2 | xargs -p -I % touch % # creates 3 files: ./1\ 2 ./3\ 4 ./5\ 6
    find . -type f -print0 | xargs -0 -I % cp % %_old
    by default the argument is placed at the end of command, with -I it can be placed somewhere else
    echo hop | xargs cp bop         # runs cp bop hop
    echo hop | xargs -I % cp % bop  # runs cp hop bop

    -r / --no-run-if-empty # do not run if input argument list is empty

    -L 1 # invoke a command for each line
    git log --format="%H %P" | xargs -L 1 git diff # diff each git commit with its parent

    -p / --interactive # prompt the user for confirmation
    -t / --verbose # prints the command

    xargs can be faster than -exec when a lot of files are involved since the right-hand side command is called only once instead of for each argument
    avoid too long list of files 'arguments list too long'

    some references
    https://en.wikipedia.org/wiki/Xargs
    http://www.cyberciti.biz/faq/linux-unix-bsd-xargs-construct-argument-lists-utility/
    http://javarevisited.blogspot.de/2012/06/10-xargs-command-example-in-linux-unix.html
    http://www.thegeekstuff.com/2013/12/xargs-examples/
    http://unixcommandstutorial.blogspot.de/2012/01/xargs.html

* cmus
  ----
    cmus # starts software

    views
    1 = artist/album view
    2 = library view
    3 = playlist view
    4 = queue view
    5 = browser
    6 = filters
    7 = settings

    Add music
    go to browser view and add to library ('a')

    Artist/album view
    space - toggles album subtree
    tab - switch between album/artist and track side

    Library view
    e - adds to queue
    y - adds to playlist

    queue/playlist editing
    p - moves below
    P - move above
    space - select: the selected track(s) can be move above or below a given track using p or P
    D - delete from list/queue

    Play modes
    s - toggle shuffle mode
    C - toggles continue mode (stop at end of track)
    r - toggles repeat mode: m sets what is repeated as indicated left of | in the status

    Play commands
    Enter / x - play
    c - pause/restart
    b - play next
    z - play prev
    v - stop

    Left/right - search forward/backward by 10s
    </>        - search forward/backward by 1min

    [/]/{/}/+/- - sound level control

    :save -p libname - saves playlist view under filename libname
    :load -p libname - loads file libname to playlist view

    man cmus-tutorialman cmus

* wget
  ----
  
    Download a site
        
        wget -r -l 2 -p http://mat.gsia.cmu.edu/classes/dynamic/dynamic.html
        
        download the page dynamic.html and all the objects it links to (-r/--recursive) up to 2 levels (-l 2) and with their requirements, e.g. included images (-p).
        A simple -r would contain -l 5, and not requirements, so the 5th link images will be missing.
        https://www.gnu.org/software/wget/manual/wget.html#Recursive-Retrieval-Options
        
* Implement a dice
  ----------------
  
    Standard D6:
    echo $(($RANDOM%6+1)) 
        http://www.cyberciti.biz/faq/bash-shell-script-generating-random-numbers/
    echo $(</dev/urandom tr -dc 1-6 | head -c1)
        http://pastebin.com/z9f422Sk
        
* date
  ----
  
    date --date="now +40 days"
    date +%Y%M%d_%H%M
    
* time
  ----
  
    time is a utility to measure the performances of programs in term of execution time, RAM, etc.
    
        aptitude install time
        /usr/bin/time -f "mem=%K RSS=%M elapsed=%E cpu.sys=%S .user=%U" program.sh
    
    Returns something like
    
        mem=0 RSS=7480 elapsed=1:33.36 cpu.sys=1.32 .user=73.70
        
    More info can be found at
    
        man 1 time
        
* read
  ----
  
    find . -type f | while read file
    do
        echo "> $file"
    done
    
    allows to work with filenames which present spaces
    