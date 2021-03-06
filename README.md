A Jira shell. Show a ticket. Create an issue. List projects.
That sort of thing.

# Getting Started

Get jirash:

    $ cd ~/opt      # or whereever
    $ git clone https://github.com/trentm/jirash.git

Put the following in your "~/.bashrc". The bash completion is just for
sub-commands. It doesn't support completing options or sub-commands
arguments.

    alias jirash='$HOME/opt/jirash/bin/jirash'      # or whatever
    complete -C 'jirash --bash-completion' jirash   # bash completion

First you need a config file with Jira URL and auth info:

    $ cat ~/.jirash.json
    {
      "jira_url": "https://dev.example.com/jira",
      "https://dev.example.com/jira": {
        "username": "joe.blow",
        "password": "secret"
      }
    }

Then use it. Note that the help output here is probably a little out of
date (i.e. there are probably more supported commands in the latest).

    $ jirash help
    Usage:
        jirash COMMAND [ARGS...]
        jirash help [COMMAND]

    Options:
        -h, --help          show this help message and exit
        --version           show version and exit
        -d, --debug         debug logging
        -J JIRA_URL, --jira-url=JIRA_URL
                            Jira base URL. Otherwise defaults to 'jira_url' value
                            from config file.

    Commands:
        createissue    Create a new issue.
        help (?)       give detailed help on a specific sub-command
        issue          Get an issue.
        issuetypes     Get an issue types (e.g. bug, task, ...).
        projects       List projects (excluding "Archived" projects).
        versions       Get available versions for the given project.

    $ jirash projects
    KEY         NAME                              LEAD
    DOC         DOC: Documentation                philip
    TOOLS       TOOLS: Tools and Extras           trent.mick
    ...

    $ jirash issues TOOLS-90
    TOOLS-90: Add a distclean target (mark.cavage -> trent.mick, Improvem...)

    $ jirash TOOLS-90     # shortcut
    TOOLS-90: Add a distclean target (mark.cavage -> trent.mick, Improvem...)

    $ jirash createissue TOOLS
    Summary: Foo is broken
    Assignee (blank for default, 'me' for yourself): me
    Description (use '.' on a line by itself to finish):
    blah
    blah
    blah
    .
    created: TOOLS-157: Foo is broken (trent.mick -> trent.mick, Bug, Normal, Open)

    $ jirash filters
    ID     AUTHOR           NAME
    10325  trent.mick       RELENG: open issues
    10389  trent.mick       TOOLS: open issues
    10183  trent.mick       trent.mick: open issues
    10104  trent.mick       trent.mick: reported issues

    # Here "TOOLS" matches the "TOOLS: open issues" saved filter.
    $ jirash issues -f TOOLS
    KEY          PRIO      STATUS    TYPE         REPORTER    ASSIGNEE    SUMMARY
    TOOLS-150    Normal    Open      New Feature  linda       trent.mick  add sdc-vminfo to operator tools
    TOOLS-143    Normal    Open      Bug          laurel      orlando     Be more tolerant of the location of the VirtualSystem location
    ...
