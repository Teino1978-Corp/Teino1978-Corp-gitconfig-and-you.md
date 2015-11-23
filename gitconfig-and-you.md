## Make `git` do stuff

Let's get some help from the shell.

```sh
$ cat >> ~/.bash_aliases
alias get = 'git'
alias gi = 'git'
alias got = 'git'
alias gti = 'git'
<ctrl+d>
$ . ~/.bash_aliases
```

## Just plain handy


```sh
$ cat ~/.gitconfig
...
[alias]
    br = branch -a
    st = status
    stat = status
    co = checkout
    ci = commit
    ls = ls-files -v
```

## Add/remove aliases like this...

```sh
$ git config --global alias.foo bar
$ git config --global alias.dir '!ls -la'
$ git foo <output>
$ git dir <output>
$ git config --global --unset alias.foo
$ git config --global --unset alias.dir
```

## Or go deeply meta with an alias for aliases

```sh
$ cat ~/.gitconfig
...
[alias]
    alias = "!sh -c '[ $# = 2 ] && git config --global alias.\"$1\" \"$2\" && exit 0 || echo \"usage: git alias <new alias> <original command>\" >&2 && exit 1' -"
    aliases = !git config --get-regexp 'alias.*' | colrm 1 6 | sed 's/[ ]/ = /'
...
```

Now you can create a new alias directly!   If you are using a shell command, make sure it's fully quoted with singles or doubles.

```sh
$ git alias foo bar
$ git alias dir '!ls -la'
```

## Check those logs

Some examples and ideas from 
- https://coderwall.com/p/euwpig
- http://durdn.com/blog/2012/11/22/must-have-git-aliases-advanced-examples/

```
lg = log --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit
lg2 = log --pretty=format:'%C(yellow)%h%Cred%d %Creset%s%Cblue [%cn]%Creset' --decorate
lg3 = log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cblue\\ [%cn]%Creset" --decorate --numstat
```

## Name it and claim it

If you put a script in your path with a name like `git-something` you can use it as a git command.

```sh
$ ls -1 ~/bin/git-*
$ git-rank   <<< ruby script
$ git-thanks <<< shell script
```

Script demos go here!


```sh
$ git thanks . <output>
$ git rank
$ git rank -o
$ git rank -h
$ git rank -v
$ emacs ~/bin/git-thanks
$ emacs ~/bin/git-rank
$...
```

Special bonus tip - ruby hates it when you have a _seemingly_ world-writable directory in the script path, e.g.

```sh
$ ll -d /afs/umich.edu/user/t
drwxrwxrwx. 29 root root 2048 Oct 28  2011 /afs/umich.edu/user/t
$ .
$ fs la /afs/umich.edu/user/t
Access list for /afs/umich.edu/user/t is
Normal rights:
  system:administrators rlidwka
  system:anyuser rl
```

But you can belay that ruby noise:

```ruby
## Suppress world-writable directory warning due to AFS nonsense.
##                      $VERBOSE
## -W0  NO Warnings     nil
## -W1  Quiet           false
## -W2  Verbose         true

BEGIN { $VERBOSE = nil }
```

## Ignoring changed files

There are several reasons you might want to ignore a file, or ignore local changes to a file.  The interesting case goes like this:

- A file is tracked in git, and needs to be, so you can't add it to .gitignore.
- Devs make local changes to that file, but should never commit/push them.  
- Counting on devs to add the file to their global `core.excludesfile` is error-prone.
- Same thing for adding the file pattern to the local repo's `.git/info/exclude`
- If that file __is **intentionally** updated__ upstream, devs should get the updates, nicely merged.

Think about app configs with db credentials needed for your work.

### Introducing... 

- `git update-index --[no-]assume-unchanged <file>`
- `git update-index --[no-]skip-worktree <file>`

[Read the docs](http://git-scm.com/docs/git-update-index) and [get the skinny](http://fallengamer.livejournal.com/93321.html).  More to come...

## Interesting git enhancements on github

- https://github.com/icefox/git-hooks (looking at this)
- https://github.com/dr4Ke/git-preserve-permissions (this too)
- https://github.com/github/hub (awesome!)

## Your turn

What makes you happy and productive with git?