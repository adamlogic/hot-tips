While some folks work in tmux all day, I just love it for tucking away long-running processes (servers, build processes, etc). These are things I need to keep running and need control over, but I want them hidden from view so they don't distract me.

To make this backgrounding flow easy, I have a shell alias `ta` (think "tmux attach") that I use exclusively to launch tmux. This command gives me a separate tmux session for each project (directory) I'm working on.

---

```shell
# .bashrc or .zshrc

alias ta='tmux attach -t ${PWD##*/} || tmux new -s ${PWD##*/}'

# This attaches an existing tmux session for the current directory
# if it exists, otherwise it starts a new one.

# The awkward `PWD##*` syntax gets the name of the current directory
# without any preceding hierarchy.
```

---

```shell
> cd project1
> ta

# The first time I run `ta` in a directory, I'll get a new tmux session.
# Detach from tmux with Ctrl-B D.

> ta

# This re-attaches me to the same session.
```

---

```shell
> cd project2
> ta

# A new tmux session is created for "project2".

> tmux list-sessions
project1: 1 windows
project2: 1 windows

# "project1" tmux session is still running, unaffected by anything
# I do in "project2".
```

---

More Info:

- [tmux cheat sheet](https://gist.github.com/andreyvit/2921703)
