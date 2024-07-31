# Tmux workflows

Tmux is a terminal multiplexer that allows you to access multiple terminal sessions within a single window. It also provides a range of features that can improve your productivity while working on the command line. In this document, we will discuss some Tmux workflows that can help you manage your terminal sessions efficiently.

## Tmux on Biowulf

Its recommended you use the `module load tmux` version instead of the default. You can add the following to your `~/.bashrc` to load it automatically:

```jsx
if [ -z ${TMUX_IS_LOADED+x} ]; then
  module load tmux
  export TMUX_IS_LOADED=1
fi 
```

Tmux cheat sheet: [https://tmuxcheatsheet.com](https://tmuxcheatsheet.com)

## Creating a new Tmux session

To create a new Tmux session, use the following command:

```
tmux new-session -s {session-name}

```

This will create a new Tmux session with the specified name. Once you have created a Tmux session, you can create multiple windows and panes within that session.

## Managing Tmux windows

To create a new Tmux window, use the following command:

```
tmux new-window

```

You can navigate between different Tmux windows using the following key combination:

```
Ctrl-b {window-number}

```

To close the current Tmux window, use the following command:

```
Ctrl-b &

```

## Managing Tmux panes

To split the current Tmux window into two panes, use the following key combination:

```
Ctrl-b %

```

To navigate between different Tmux panes, use the following key combination:

```
Ctrl-b arrow-key

```

To resize a Tmux pane, use the following key combination:

```
Ctrl-b Alt-arrow-key

```

## Managing Tmux sessions

To list all Tmux sessions, use the following command:

```
tmux list-sessions

```

To attach to a Tmux session, use the following command:

```
tmux attach-session -t {session-name}

```

To detach from a Tmux session, use the following key combination:

```
Ctrl-b d

```

## Conclusion

Tmux is a powerful tool that can help you manage your terminal sessions efficiently. By using the workflows discussed in this document, you can improve your productivity and make the most of Tmux's features.

---

References: [https://www.redhat.com/sysadmin/introduction-tmux-linux](https://www.redhat.com/sysadmin/introduction-tmux-linux)

Note: Notion AI wrote this
