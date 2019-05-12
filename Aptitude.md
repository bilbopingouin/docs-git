# aptitude search output


e.g. 

```bash
aptitude search cairo -F %4c%p%v%V%d
```

adds the current version number and the possible version update. See [Ref.](http://aptitude.alioth.debian.org/doc/en/ch02s05s01.html).


# aptitude search patterns

e.g.
```bash
aptitude search ~U			# search for updates
aptitude search ?exact-name\(package\)
aptitude search ~dpattern		# description matches pattern
```

See [Ref.](http://aptitude.alioth.debian.org/doc/en/ch02s04s05.html).

# aptitude configuration file


    write
    Aptitude {
      CmdLine {
	Package-Display-Format "%c%a%M %p %v%V - %d";
      }
    }
    in ~/.aptitude/config
    It will be overwritten to
        Aptitude "";
        Aptitude::CmdLine "";
        Aptitude::CmdLine::Package-Display-Format "%c%a%M %p %v%V - %d";
    But it changes the output. Further options can be found in
    http://aptitude.alioth.debian.org/doc/en/ch02s05s05.html

# aptitude download


    aptitude -d install <package> # download packages only in /var/cache/apt/archives
    apt-get -d -o=dir::cache=/tmp install <package> # sets the place where it is downloaded
    or
    apt-get download -o=dir::cache=/tmp <package>
    https://stackoverflow.com/questions/4419268/how-do-i-download-a-package-from-apt-get-without-installing-it

# backports


    add
    deb http://http.debian.net/debian wheezy-backports main
    to /etc/apt/sources.list
    aptitude update
    aptitude -t wheezy-backports search git # shows a newer version
    aptitude -t wheezy-backports install git # gets a newer version
    http://backports.debian.org/Instructions/

# aptitude UI

  
    Running
    
        aptitude
        
    starts a UI.
    
    Q           quit
    q           close current view
    u           same as `aptitude update`
    U puis g    same as `aptitude safe-upgrade`
    +           mark a single (all = U) package to be installed
    -           remove a package
    =           hold a package in its current version (prevent future upgrades)
    :           keep a package in its current version (allows future upgrades)
    <C-u>       undo the last action
    ?           display help
    <F6>        next tab
    <F7>        previous tab
    
