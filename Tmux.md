* Bleeding
  --------

    One problem when having the default TERM set to xterm-256color, one can see a "bleeding" in some programs.

    In vim, one can solve that problem by having a :set t_ut= (e.g. in .vimrc) and C-L to update the page.
    Other programs like aptitude (in interactive mode) may not have that option. One way to get it right is, e.g.
    TERM=screen-256color aptitude
    which do not change the configuration for the rest just for this command.
    https://sunaku.github.io/vim-256color-bce.html

* Prefix
  ------
  
    By default, the prefix is set to <C-b> It can be changed by using
    
    set -g prefix C-a
    
    in the configuration file
    
* Multiple panes
  --------------
  
    By default, panes splitting is obtained by 
    
        <C-b> % # for a vertical split
        <C-b> " # for an horizontal split
        
    Navigation
    
        <C-b> <Arrow key>
        
        or
        
        <C-b> q <number>
        
    Closing a pane
    
        <C-d>
        
    http://www.devx.com/enterprise/manage-multiple-interactive-sessions-with-tmux.html
    
* Inline help
  -----------
  
    <C-b> ?
    
* Nested session
  --------------
  
    `tmux` has a built-in protection against nested session. If from one session, one tries to attach or create a new session, it prints the following
    
        sessions should be nested with care. unset $TMUX to force.
        
    But to get another session on the side, one could do the following
    
        <C-b> %     # splits the window
        bash        # starts a new subshell
        TMUX=''     # unset TMUX
        tmux attach -t session_name # attach the session or create a new one
        
    It should be noted that in some environment, when running TMUX='' it gets completely disconnected from the original tmux server. So creating a new session works, but not attaching a same level session.
  
    To remove the side nested session, one can
    
        <C-b> <C-b> d   # detaches the nested session <C-b> <C-b> sends the prefix to the nested session
        <C-d>           # leaves the subshell. So TMUX keeps the old value you might consider `exit`
        <C-d>           # closes the pane
        
    An alternative would be to store the content of TMUX into another variable to be able to restore it later.
        https://unix.stackexchange.com/questions/107551/re-set-tmux-after-unsetting
        
* Switching between sessions
  --------------------------
  
    Select the session
    
        <C-b> s
        
        or 
        
        <C-b> : attach-session -t name
        
    Cycle through
    
        <C-b> )
        <C-b> (
        
    https://stackoverflow.com/questions/16398850/create-new-tmux-session-from-inside-a-tmux-session
    
    Create a new session from within a session
    
        <C-b> : new [-s name]
        
* Rename a session
  ----------------
  
    Rename the current session with
    
        tmux rename-session new-name
    
    or
    
        <C-b> $
        
* Windows
  -------
  
    Create a new window within a session
    
        <C-b> c
        
    Rename a window
    
        <C-b> ,
        
    Cycle through windows
    
        <C-b> n
        <C-b> p
        
    by default.
    
    
* Scrolling and copy-mode
  -----------------------
  
    Enter copy mode

        <C-b> [
        
    Leave copy mode
    
        q
        
    With the configuration below
        <C-b> [
        (arrows)
        v
        (arrows, 0, $, ...)
        y
        q
        <C-b> p
    
* Configuration
  -------------
  
    This is the configuration file that I use ~/.tmux.conf
    
        # http://www.openbsd.org/cgi-bin/man.cgi/OpenBSD-current/man1/tmux.1?query=tmux&sec=1
        # https://gist.github.com/anonymous/6bebae3eb9f7b972e6f0
        #Prefix is Ctrl-a
        # I somehow got used to the C-b thingy...
        #set -g prefix C-a
        #bind C-a send-prefix
        #unbind C-b

        # Time between two characters 
        set -sg escape-time 1

        # Windows are numbered from 1 (and not 0)
        set -g base-index 1

        # pane are numbered from 1 (and not 0)
        setw -g pane-base-index 1

        #Mouse works as expected
        # That seems to spoil the typical mouse behaviour. Who need a mouse anyways ;-)
        # enters copy mode allows drag and drop and wheel scrolling (while entering copy-mode)
        #setw -g mode-mouse on
        #set -g mouse-select-pane on
        #set -g mouse-resize-pane on
        #set -g mouse-select-window on

        # Highlight windows with activity in the statusline
        setw -g monitor-activity on

        # display a message when some activity occurs
        set -g visual-activity on

        # in copy-mode use vim keys
        set -g mode-keys vi

        #number of lines in windows history
        set -g history-limit 10000

        # y and p as in vim
        bind Escape copy-mode
        unbind p
        bind p paste-buffer
        bind -t vi-copy 'v' begin-selection
        bind -t vi-copy 'y' copy-selection
        #bind -t vi-copy 'Space' halfpage-down 
        #bind -t vi-copy 'Bspace' halfpage-up
        # previous two commands not available in tmux 1.6

        # extra commands for interacting with the ICCCM clipboard
        bind C-c run "tmux save-buffer - | xclip -i -sel clipboard"
        bind C-v run "tmux set-buffer \"$(xclip -o -sel clipboard)\"; tmux paste-buffer"

        # easy-to-remember split pane commands
        bind | split-window -h
        bind - split-window -v
        unbind '"'
        unbind %
        # moving between panes with vim movement keys
        bind h select-pane -L
        bind j select-pane -D
        bind k select-pane -U
        bind l select-pane -R
        # moving between windows with vim movement keys
        bind -r C-h select-window -t :-
        bind -r C-l select-window -t :+
        # resize panes with vim movement keys
        bind -r H resize-pane -L 5
        bind -r J resize-pane -D 5
        bind -r K resize-pane -U 5
        bind -r L resize-pane -R 5

        setw -g xterm-keys on
        set -g default-terminal "$TERM"

        #http://dev.gentoo.org/~wired/conf/tmux.conf
        ## ` is an interesting key for a prefix
        #set-option -g prefix `
        ## set-option -g prefix C-a
        #
        #unbind-key C-b
        #bind-key C-a last-window
        #bind-key ` last-window
        #bind-key a send-prefix
        #
        ## we might need ` at some point, allow switching
        ## we can also send the prefix char with `-a
        #bind-key F11 set-option -g prefix C-a
        #bind-key F12 set-option -g prefix `
        #
        ## 0 is too far from ` ;)
        #set -g base-index 1
        #
        ## set-option -g default-terminal "screen-256color"
        #set-option -g mouse-select-pane on
        #set-option -g status-keys vi
        #set-option -g bell-action any
        #set-option -g set-titles on
        #set-option -g set-titles-string '#H:#S.#I.#P #W #T' # window number,program name,active (or not)
        #set-option -g visual-bell off
        #
        #setw -g mode-keys vi
        #setw -g mode-mouse on
        #setw -g monitor-activity on
        #
        #bind e previous-window
        #bind f next-window
        #bind E swap-window -t -1
        #bind F swap-window -t +1
        ## bind j up-pane 
        ## bind k down-pane
        #
        #set-option -g status-utf8 on
        ## set-option -g status-justify centre
        #set-option -g status-justify left
        #set-option -g status-bg black
        #set-option -g status-fg white
        #set-option -g status-left-length 40
        #set-option -g status-right-length 80
        #
        #set-option -g pane-active-border-fg green
        #set-option -g pane-active-border-bg black
        #set-option -g pane-border-fg white
        #set-option -g pane-border-bg black
        #
        #set-option -g message-fg black
        #set-option -g message-bg green
        #
        ##setw -g mode-bg black
        #
        #setw -g window-status-bg black
        #setw -g window-status-current-fg green
        #setw -g window-status-bell-attr default
        #setw -g window-status-bell-fg red
        #setw -g window-status-content-attr default
        #setw -g window-status-content-fg yellow
        #setw -g window-status-activity-attr default
        #setw -g window-status-activity-fg yellow
        #
        #set -g status-left '#[fg=red]#H#[fg=green]:#[fg=white]#S #[fg=green]][#[default]'
        #
        ## set -g status-right '#[fg=green]][#[fg=white] #T #[fg=green]][ #[fg=blue]%Y-%m-%d #[fg=white]%H:%M#[default]'
        #
        #set -g status-interval 5
        #set -g status-right '#[fg=green]][#[fg=white] #(tmux-mem-cpu-load 5 4) #[fg=green]][ #[fg=yellow]%H:%M#[default]'
        #
        #set -g history-limit 4096
        #
        #bind r source-file ~/.tmux.conf

        # mark the active window in red to make it more clear than the *
        setw -g window-status-current-fg red
        
* Resources
  ---------
  
    https://tmuxp.readthedocs.org/en/latest/about_tmux.html
    https://gist.github.com/MohamedAlaa/2961058
    https://robots.thoughtbot.com/a-tmux-crash-course
    https://wiki.archlinux.org/index.php/Tmux