# Jump to tagged messages                                                                                    [35/324]  -----------------------

can be done using `/~T` or in `.muttrc`

    macro index ,T '<search>~T<return>'

# Internal address book

In `.muttrc`

    set alias_file=~/.mutt/aliases
    source ~/.mutt/aliases

In `~/.mutt/aliases`

    alias <nickname> <Long Name> <email@address.com>

Write a new mail as

    m
    nick
    <Tab>

# External address book

Install abook

    aptitude install abook

Configure `.muttrc`

    set query_command= "abook --mutt-query '%s'"
    macro generic,index,pager \ca "<shell-escape>abook<return>" "launch abook"
    macro index,pager A "<pipe-message>abook --add-email-quiet<return>" "add the sender address to abook"

Usage

- `A`               adds the current sender to the address book (`~/.abook/addressbook`)
- `<C-A>`           launch the address book from within mutt (`m` to send an email to current entry)
- `Q`               search in the address book

# Move several emails

In two steps:

- Tag emails

    macro index ,fF 'T~f "(facebook\.com|facebookmail\.com)"<enter>'

- Move emails

    macro index ,mF ';s=Social /Facebook'

It's possible to do it one step, but if no email is tagged, the moving command will move the current email.

# Search flags

See the [list of patterns](http://www.mutt.org/doc/manual/#patterns)

- `~N`              Search next new message
- `~b <pattern>`    Search pattern in body
- `~e <pattern>`    Search pattern in sender line
- `~T`              Search next tagged message
- `~F`              Search next flagged message
- `~f <name>`       Search next message with `<name>` in the `From:` line
- `~t <name>`       Search next message with `<name>` in the `To:` line
- `~s <pattern>`    Search next message with `<pattern>` in the `Subject:` line


Combinations

    ~U (~z 1M- | ~f johndoe)

matches all unread emails with either size larger than 1MB or from `johndoe`

# Draft

Instead of sending (composing, exiting editor and `y`), one can use `P` to Postpone a message (after exiting the editor).

A postponed message can be recalled from the index using `R`.

# Split screen

To see the index even when reading a mail, one can use

    :set pager_index_lines=10

Which will keep 10 lines of the index, when reading the other emails.

# Colours emails in index

For example

    color index blue default '~f "meetup.com"'

# Create a new folder

We have the following folder commands
- `c`           change folder
- `C`           create a new folder

See also [How do I create a new folder](https://superuser.com/questions/154060/how-do-i-create-a-new-folder-in-mutt#154086).
